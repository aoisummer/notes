# Web 监控视频的串流方案

在含有视频监控的项目中，在 Web 端观看是一个不算少见的需求。一般来说硬件厂商会自带一个客户端之类的界面用于简易观看，但进行 Web 开发时都会进行额外定制。

厂商对于设备的流媒体传输一般只支持 RTSP，早期都是提供 ActiveX 控件或 NPAPI 插件，但前者已随着 IE 的停止支持而逐渐不被操作系统支持，后者则是因安全问题被 Chrome 等放弃。这意味着浏览器端已没有任何直接支持该协议的手段（让用户直连设备也有点安全性问题），于是需要找到一些方案用于解决用户观看的问题。

:::info

本文的操作以 Windows 系统为主，但使用的工具基本都是跨平台，因此你可以方便地在其它系统上复现。

:::

## 前言 {#foreword}

本处不会详细讲解每个概念，但会列出一些经常被误解和混淆的概念：

- **视频格式**：通过压缩技术将视频原始数据中的冗余信息去除，降低数据大小以便传输与存储
  - H.264（MPEG-4 AVC）
  - H.265（MPEG-H HEVC）
  - AV1
  - VP9
  - RealVideo
  - Xvid
  - VC-1
  - Windows Media Video
  - Apple Video（QuickTime Video）
- **视频编码器**：实现上述格式的工具
  - FFmpeg
  - x264
  - x265
- **音频格式**：通过压缩技术将音频原始数据中的冗余信息去除，降低数据大小以便传输与存储
  - MP3
  - AAC
  - Opus
  - AC-3
  - ALAC
  - FLAC
  - Monkey's Audio
  - WavPack
  - Windows Media Audio
- **音频编码器**：实现上述格式的工具
  - FFmpeg
  - LAME
  - Apple AAC
  - FDK AAC
  - Nero AAC
- **容器**：将音视频（也可能含字幕等其它数据）包装在一个文件中，以方便播放
  - MP4（MPEG-4 Part 14）
  - TS（MPEG-TS）
  - MPG（MPEG-PS）
  - AVI
  - Matroska、WebM
  - Ogg
  - RealMedia
  - Flash Video
  - VOB
  - MOV（QuickTime）
- **容器工具**：实现上述格式的工具
  - FFmpeg
  - VirtualDub
  - MP4Box
  - MKVToolNix
  - HandBrake

## 准备

鉴于不是随时都能找到一个设备进行调试，因此准备一个本地的 RTSP 源比较方便，这里会用到本文的主要工具 [MediaMTX](https://github.com/bluenviron/mediamtx) 作为 RTSP 服务器端。

:::note

在 Linux 下，你也可以使用 [GStreamer](https://gstreamer.freedesktop.org/) 作为 RTSP 服务器端。

:::

该服务器端默认的 RTSP 端口为 8554，准备一个常见格式的 MP4 文件如 `input.mp4` 作为“直播源”，使用 FFmpeg 循环推送到该服务器端上以发布该源：

```
ffmpeg -re -stream_loop -1 -i input.mp4 -vcodec libx264 -s 854x480 -b:v 1000k -bf 0 -an -f rtsp rtsp://127.0.0.1:8554/mystream
```

你可以查阅 FFmpeg 帮助修改参数以符合你的需求。

现在我们有了一个 RTSP 源，地址是 `rtsp://127.0.0.1:8554/mystream`，为了测试该地址是否生效，你可以使用支持的客户端打开该地址进行播放，例如：VLC、PotPlayer 等。

## 流媒体协议 {#streaming-protocol}

这是一种将多媒体数据分段压缩后，通过网络实时发送以供用户观看的应用层协议。常见的协议有：

### [RTSP](https://zh.wikipedia.org/wiki/%E5%8D%B3%E6%99%82%E4%B8%B2%E6%B5%81%E5%8D%94%E5%AE%9A)

早期的实现标准，有大量客户端支持，但浏览器均不支持。

### [RTMP](https://zh.wikipedia.org/wiki/%E5%AE%9E%E6%97%B6%E6%B6%88%E6%81%AF%E5%8D%8F%E8%AE%AE)

由 Macromedia（后为 Adobe）为 Flash 开发的一个专有协议，浏览器需要 Flash 插件才能支持（Flash 已被放弃，浏览器也逐渐停止对其的支持）。

### [HLS](https://zh.wikipedia.org/wiki/HTTP_Live_Streaming)

由苹果开发，基于 HTTP 协议的实现，在移动端上原生支持度高。

### [WebRTC](https://zh.wikipedia.org/wiki/WebRTC)

这本来是一个设计用于浏览器端进行实时语音、视频对话的现代 API，但也可用来传输直播流。

## 技术选择 {#tech-choice}

总结一下上面的协议可以得到下表：

| 设备 | 流媒体协议 | 传输协议  | 渲染               |
| ---- | ---------- | --------- | ------------------ |
| RTSP | RTSP、RTMP | TCP       | 浏览器插件         |
|      | HLS        | HTTP      | 基于浏览器、hls.js |
|      | WebRTC     | TCP、UDP  | 基于浏览器         |
|      | -          | WebSocket | jsmpeg、flv.js     |

其中 RTSP、RTMP 基本不被浏览器支持，因此不予选择。非 RTSP 的情况，需要一个媒体服务器端对原始协议进行转换、代理为其它协议类型，在前面的准备阶段提到的 MediaMTX 可以完成这项任务。如果你已有一个设备可以用于测试，可以参考它的文档修改配置文件 `mediamtx.yml` 加入你的设备，例如：

```yaml
paths:
  mystream:
    source: rtsp://abc:123@192.168.1.123:554/streaming
```

针对后几种情况，下面会进行测试。

### 基于 HLS {#based-on-hls}

通过[查询](https://caniuse.com/http-live-streaming)可以发现，该协议对于桌面端的原生支持不好，但对于非原生支持的浏览器可以通过 [HLS.js](https://github.com/video-dev/hls.js) 库实现播放，下面以该库作为播放器进行。

MediaMTX 支持以 HLS 协议访问 RTSP 源，默认通过 8888 端口访问该服务，编辑如下文件即可访问前面准备的源：

```html title="hls.html"
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <script src="https://unpkg.com/hls.js@1.4.8/dist/hls.min.js"></script>
  </head>
  <body>
    <video muted></video>
    <script>
      var video = document.querySelector('video');
      var hls = new Hls();
      hls.attachMedia(video);
      hls.on(Hls.Events.MEDIA_ATTACHED, function () {
        hls.loadSource('http://127.0.0.1:8888/mystream/index.m3u8');
      });
      video.play();
    </script>
  </body>
</html>
```

本协议实施比较方便，浏览器兼容性也较好，但主要缺点是延迟较高，如果对延迟方面有要求则本方案可能不适合。

### 基于 WebRTC {#based-on-webrtc}

作为一个比较新的标准，WebRTC 的[兼容性](https://caniuse.com/rtcpeerconnection)比起 HLS 来说要差一点。而且该协议并非专门为直播流设计，大多数为底层 API，高层封装的库要依据具体实现而各自编写。

MediaMTX 也支持以 WebRTC 方式访问 RTSP 源，默认通过 8889 端口访问该服务，编辑如下文件即可访问前面准备的源：

```html title="webrtc.html"
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <script src="https://unpkg.com/webrtc-adapter@8.2.3/out/adapter.js"></script>
  </head>
  <body>
    <iframe
      width="854"
      height="480"
      style="border: 0;"
      src="http://127.0.0.1:8889/mystream"
    ></iframe>
  </body>
</html>
```

:::note

这里引入了 WebRTC adapter，用于解决各浏览器对 WebRTC 支持的差异性问题。

:::

此处以一个 `iframe` 的形式内部嵌入了一个 `video` 标签，内部封装库由内置实现，外部控制不方便，但大多数情况下仅插入该标签即可，实施方便，延迟也较低。

### 基于 WHEP {#based-on-whep}

WHEP（WebRTC HTTP Egress Protocol）是一个全新的、扩展自 WebRTC 的协议标准草案，改善了前面提到的 WebRTC 全是底层 API，没有高层信令协议的问题。

MediaMTX 也支持以 WHEP 方式访问 RTSP 源，编辑如下文件即可访问前面准备的源：

```html title="whep.html"
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <script src="https://unpkg.com/webrtc-adapter@8.2.3/out/adapter.js"></script>
  </head>
  <body>
    <video autoplay muted></video>
    <script type="module">
      import WHEPClient from 'https://fastly.jsdelivr.net/gh/cloudflare/workers-sdk@main/templates/stream/webrtc/src/WHEPClient.js';

      const url = 'http://127.0.0.1:8889/mystream/whep';
      const videoElement = document.querySelector('video');
      const client = new WHEPClient(url, videoElement);
    </script>
  </body>
</html>
```

:::note

这里使用了由 Cloudflare 提供的 WHEP 客户端库，你可以从他们的 [Github](https://github.com/cloudflare/workers-sdk/tree/main/templates/stream/webrtc/src) 上下载那些文件用于本地部署。

:::

本方案因为是基于 WebRTC，所以优点是相同的，实施也比较方便，但由于是新的标准，具体兼容性要进行实验才能得知。

### 基于 WebSocket {#based-on-websocket}

WebSocket 虽然不是流媒体协议，但其全双工的通信方式也可以用于向客户端推送多媒体数据。且由于良好的浏览器支持度，以及可以与 HTTP 复用端口，可以绕过前面的部分协议中对防火墙限制的需求。

因为没有在其之上的标准，所以服务器端如何实现全凭开发者自行决定，也就没有对应的标准库可以使用，但下面也提供一些实现方式以供参考：

- [RTSP Relay](https://github.com/k-yle/rtsp-relay)：这是一个基于 Express 的服务器端，内部使用了 FFmpeg 进行转码为 MPEG-1 视频格式，在浏览器端使用 jsmpeg 播放。缺点是 MPEG-1 格式压缩率太低，比较占用带宽。
- [flv.js](https://github.com/Bilibili/flv.js)：本来是一个不需要 Flash 插件支持，用于播放 Flash Video 的浏览器端播放器，后扩展为可以支持通过 WebSocket 传输 FLV 流播放。不含服务器端。

## 其它方案 {#other}

### [WebRTC-Streamer](https://github.com/mpromonet/webrtc-streamer)

这是一个可以转换多种协议到 WebRTC 流方式访问的服务器端，还内置了 STUN、TURN 用于解决 WebRTC 的一系列问题。

示例：

```html title="webrtc-streamer.html"
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <script src="https://unpkg.com/webrtc-adapter@8.2.3/out/adapter.js"></script>
    <script src="https://unpkg.com/webrtc-streamer@0.8.1/html/webrtcstreamer.js"></script>
  </head>
  <body>
    <video id="video" muted></video>
    <script>
      var server = new WebRtcStreamer('video', 'http://127.0.0.1:8000');
      server.connect('rtsp://127.0.0.1:8554/mystream');
    </script>
  </body>
</html>
```

这种方式实施起来也比较方便，尤其是可以通过浏览器端自定 RTSP 源，也是推荐的方式。

本服务器端同样支持最新的 WHEP 协议，可以使用如下示例进行访问：

```html title="webrtc-streamer-whep.html"
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <script src="https://unpkg.com/webrtc-adapter@8.2.3/out/adapter.js"></script>
    <script src="https://unpkg.com/@eyevinn/whep-video-component@latest/dist/whep-video.component.js"></script>
  </head>
  <body>
    <whep-video
      src="http://127.0.0.1:8000/api/whep?url=rtsp%3A%2F%2F127.0.0.1%3A8554%2Fmystream"
      muted
      autoplay
    ></whep-video>
  </body>
</html>
```

## 总结 {#conclusion}

前面列出了很多方案，综合来看，WebRTC 是延迟上比较有优势的，是推荐方案，但具体实现各有千秋。WHEP 作为最新标准仍在草案阶段，有可能兼容性问题不满足需求，需要慎用。
