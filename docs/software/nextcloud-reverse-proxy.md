# Nextcloud 的反向代理配置

默认情况下，官方似乎对非标准端口（80/443）特别讨厌，重定向总是会出现各种问题。但是若需要公网使用非标准端口的 https 反向代理，局域网使用 http，则需要一定的配置。

示例的环境如下，请根据你的实际情况调整：

- 局域网地址：`http://192.168.1.101:8000`
- 公网地址：`https://nextcloud.example.com:10443`

## 配置 nginx {#nginx-config}

```
server {
    listen 10443 ssl;
    http2 on;
    server_name  nextcloud.example.com;

    # 省略证书部分

    location / {
        proxy_pass http://192.168.1.101:8000;

        proxy_set_header Host $host:10443;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header X-Forwarded-Port 10443;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## 配置 nextcloud {#nextcloud-config}

这里的核心是修改 `/var/www/html/config/config.php` 文件，docker 用户请调整你的文件路径。

```php
<?php
$protocol = (isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https') ? 'https' : 'http';

$CONFIG = array (
  // 省略部分
  'trusted_proxies' =>
    array (
      0 => '192.168.1.1', // 反向代理服务器地址
    ),
  'trusted_domains' =>
    array (
      0 => '192.168.1.101:8000',
      1 => 'nextcloud.example.com:10443',
      2 => 'nextcloud.example.com',
    ),
  'overwrite.cli.url' => 'http://192.168.1.101:8000',
  'overwriteprotocol' => $protocol,
);
```
