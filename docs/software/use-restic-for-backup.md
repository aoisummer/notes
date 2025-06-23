# 使用 Restic 进行数据备份

[Restic](https://github.com/restic/restic) 是一个由 Go 编写的跨平台备份程序，支持多种位置作为备份后端，例如：本地目录、sftp、HTTP REST server、亚马逊、微软、谷歌云等、甚至一个 Rclone 连接也可以。它对所有数据都会进行加密和验证，以确保数据安全性。

## 安装 {#install}

### 服务端 {#server-side}

本文使用同一开发者发布的一个简易 [HTTP REST server](https://github.com/restic/rest-server) 作为服务端，该 REST server 同样也为跨平台程序，所有系统均有支持。

下载对应平台预编译包：https://github.com/restic/rest-server/releases ，准备好程序以后使用命令启动服务：

```
rest-server --path "<数据目录>" --private-repos
```

程序默认监听在 8000 端口，访问地址为 `http://<IP地址>:8000`。第二个参数启用了用户隔离访问目录，例如用户 `test` 仅可访问 `http://test:<密码>@<IP地址>:8000/test`。

数据目录里需要一个 `.htpasswd` 文件来保存验证的用户列表，可以使用 `htpasswd` 程序或在线工具生成用户名/密码

```
htpasswd -B -c .htpasswd <用户名>
```

### 客户端 {#client-side}

本文使用 Windows 作为客户端。

- 使用 Scoop 包管理器：`scoop install restic`
- 手动下载对应平台预编译包：https://github.com/restic/restic/releases ，然后将可执行文件放置到 `%PATH%` 可访问的目录里

考虑到 Restic 参数众多，这里使用一个带 UI 的应用来进行备份管理：[Backrest](https://github.com/garethgeorge/backrest)

- 使用 Scoop 包管理器：`scoop install extras/backrest`
- 手动下载对应平台预编译包：https://github.com/garethgeorge/backrest/releases ，解压使用

运行 `backrest-windows-tray.exe` 启动任务栏图标，右击图标选择 Open WebUI 即可进行管理，它会自动检测当前 `%PATH%` 内的 restic 可执行文件。

## 设置备份 {#set-backup-plan}

打开 Backrest WebUI，在左侧的版本库列表里新增一个前面建立的库，在 URI 处填写包含用户名和用户密码的全字符串，如：`rest:http://<用户名>:<用户密码>@<IP地址>:8000/<用户名>`，最后在库密码处填入加密密码以提交保存，程序会自动在该路径上进行初始化操作。

然后在左侧的计划列表里新增一个备份计划，选择上一步新建的库，之后在路径里填入需要备份的路径，可以填入多个以同时备份多个路径。Backrest 会自动生成计划备份任务，默认为每小时进行处理，该任务在 Backrest 进程运行时会自动处理，不需手动加入系统计划任务。

计划建立成功后会按计划任务执行备份，如果需要立刻备份需要点击上面的 `Backup Now` 按钮，这会立刻进行备份操作。

## 还原备份 {#restore-from-repo}

### 命令行 {#command-line}

Backrest 目前还没有包含还原的功能，需要使用 restic 命令行进行操作：

```
restic -r <版本库URI> restore latest --target "<还原目标目录>" --path "<备份路径>"
```

这条命令会将指定路径的最新更改还原到目标目录中。

### Restic Browser

这是一个 Restic 的版本库浏览和恢复工具，可以方便浏览文件历史版本，并还原单个文件。

- 使用 Scoop 包管理器：`scoop install extras/restic-browser`
- 手动下载对应平台预编译包：https://github.com/emuell/restic-browser/releases ，解压使用

## 更多功能和使用指南 {#more}

官方文档：https://restic.readthedocs.io/en/latest
