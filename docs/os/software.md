---
tags:
  - windows
---

# 常用软件

本处收集的软件以 Windows 为主，部分包含 Scoop 包名。

## 软件包管理 {#package-management}

| 名称                                              | 简介                                                                     |
| ------------------------------------------------- | ------------------------------------------------------------------------ |
| [winget](https://github.com/microsoft/winget-cli) | Windows 程序包管理器是微软为 Windows 10 开发的一款自由开源的软件包管理器 |
| [scoop](https://scoop.sh/)                        | Scoop 是一个 Windows 的命令行安装工具，推荐用于偏绿色的软件              |

## 浏览器 {#browser}

| 名称                                              | 简介                                        | Scoop               |
| ------------------------------------------------- | ------------------------------------------- | ------------------- |
| [Firefox](https://www.mozilla.org/zh-CN/firefox/) | 由 Mozilla 公司开发，自由及开源的网页浏览器 | extras/firefox      |
| [Chrome](https://www.google.cn/chrome/)           | Google 开发的免费网页浏览器                 | extras/googlechrome |

## 压缩解压 {#archive}

| 名称                            | 简介                                         | Scoop |
| ------------------------------- | -------------------------------------------- | ----- |
| [7-Zip](https://www.7-zip.org/) | 一个支持多种格式的归档管理器，有很高的压缩比 | 7zip  |

## 编程开发 {#programming}

| 名称                                                          | 简介                                              | Scoop                                          |
| ------------------------------------------------------------- | ------------------------------------------------- | ---------------------------------------------- |
| [Notepad++](https://notepad-plus-plus.org/)                   | 一个 Windows 上免费的源码编辑器，支持多种语言高亮 | extras/notepadplusplus                         |
| [Visual Studio Code](https://code.visualstudio.com/)          | 一款由微软开发且跨平台的免费源代码编辑器          | extras/vscode                                  |
| [Git](https://git-scm.com/)                                   | 一个免费开源的分布式文件版本控制系统              | git                                            |
| [TortoiseGit](https://tortoisegit.org/)                       | 在 Windows 外壳上提供一个 Git 的用户操作界面      |                                                |
| [TortoiseSVN](https://tortoisesvn.net/)                       | 在 Windows 外壳上提供一个 SVN 的用户操作界面      |                                                |
| [NVM for Windows](https://github.com/coreybutler/nvm-windows) | 用于 Windows 的 Node.js 的多版本管理器            | nvm                                            |
| [Temurin JRE / JDK](https://adoptium.net/)                    | Eclipse 维护的预编译 Java 运行时                  | java/temurin-lts-jre<br />java/temurin-lts-jdk |

## 网络软件 {#network}

| 名称                                                                                    | 简介                                                        | Scoop                       |
| --------------------------------------------------------------------------------------- | ----------------------------------------------------------- | --------------------------- |
| [Free Download Manager](https://www.freedownloadmanager.org/)                           | 一款强大又现代的下载加速器，支持 HTTP/HTTPS/FTP/BT          | extras/freedownloadmanager  |
| [qBittorrent Enhanced Edition](https://github.com/c0re100/qBittorrent-Enhanced-Edition) | 开源的 BT 客户端，基于 qBittorrent 的增强版，支持反吸血功能 | extras/qbittorrent-enhanced |
| [OBS Studio](https://obsproject.com/)                                                   | 免费开源的直播和视频录制软件                                | extras/obs-studio           |
| [Thunderbird](https://www.thunderbird.net/)                                             | 一款免费的电子邮件应用程序，配置简单，定制自由              | extras/thunderbird          |
| [QuiteRSS](https://quiterss.org/)                                                       | 一个开源的本地 RSS 阅读器                                   | extras/quiterss             |

## 多媒体 {#multimedia}

| 名称                                                         | 简介                                                                                         | Scoop             |
| ------------------------------------------------------------ | -------------------------------------------------------------------------------------------- | ----------------- |
| [PotPlayer](https://potplayer.daum.net/)                     | 一个免费的媒体播放器，内置多种格式解码器，支持硬件加速，多种字幕格式                         |                   |
| [K-Lite Codec Pack](https://www.codecguide.com/about_kl.htm) | 编解码器包是一个包含了大量音频视频格式的编码解码器包，外挂的解码器也可以兼容其它第三方播放器 |                   |
| [foobar2000](https://www.foobar2000.org/)                    | 一个 Windows 上的免费高级音频播放器                                                          | extras/foobar2000 |
| [XnView MP](https://www.xnview.com/en/xnviewmp/)             | 一个强大的图片管理浏览器，支持批量裁剪和转换                                                 | extras/xnviewmp   |
| [Honeyview](https://www.bandisoft.com/honeyview/)            | 一款轻量快速的图片查看器                                                                     | extras/honeyview  |

## 硬件工具 {#hardware}

| 名称                                                         | 简介                                  | Scoop                  |
| ------------------------------------------------------------ | ------------------------------------- | ---------------------- |
| [CPU-Z](https://www.cpuid.com/softwares/cpu-z.html)          | 免费的系统硬件信息工具，以 CPU 为主   | extras/cpu-z           |
| [GPU-Z](https://www.techpowerup.com/gpuz)                    | 显示显卡和 GPU 各种信息的轻量级的工具 | extras/gpu-z           |
| [CrystalDiskInfo](https://osdn.net/projects/crystaldiskinfo) | 显示硬盘信息的工具                    | extras/crystaldiskinfo |
| [HWiNFO](https://www.hwinfo.com/)                            | 专业的系统信息、硬件分析与监控软件    | extras/hwinfo          |
| [Ventoy](https://www.ventoy.net/)                            | 创建可引导 USB 设备的工具             | extras/ventoy          |

## 备份 {#backup}

| 名称                                                 | 简介            | Scoop           |
| ---------------------------------------------------- | --------------- | --------------- |
| [Restic](https://restic.net/)                        | 现代的备份程序  | restic          |
| [Backrest](https://github.com/garethgeorge/backrest) | Restic 的 WebUI | extras/backrest |

## 其它 {#other}

| 名称                                                 | 简介                                            | Scoop           |
| ---------------------------------------------------- | ----------------------------------------------- | --------------- |
| [RapidCRC](https://ov2.eu/programs/rapidcrc-unicode) | Hash 计算工具                                   | extras/rapidcrc |
| [WinSCP](https://winscp.net)                         | 支持 FTP、SFTP、WebDAV 等多种协议的文件传输工具 | extras/winscp   |
| [nssm](https://nssm.cc)                              | 自定义的 Windows 服务管理器                     | nssm            |
