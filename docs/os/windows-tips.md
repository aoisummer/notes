---
tags:
  - windows
---

# Windows 使用技巧

:::info

部分命令需要管理员权限。

:::

## 关闭默认共享 {#close-default-shares}

默认情况下，Windows 会隐藏共享所有磁盘所在分区（可以通过 `net share` 命令查看），以及 Windows 安装目录（`%SYSTEMROOT%`）。出于某些安全考虑可关闭这些默认共享。

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters /v AutoShareServer /t REG_DWORD /d 0 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters /v AutoShareWks /t REG_DWORD /d 0 /f
```

## 启用长路径支持 {#enable-long-path}

默认情况下，对于文件路径长度，Windows 仅支持 260 个字符，这可能在使用某些过长文件名时遇到问题。启用这项设置可解决这类问题。

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem /v LongPathsEnabled /t REG_DWORD /d 1 /f
```

## 停止系统大版本升级 {#disable-os-upgrade}

停止对于过时旧系统的升级提示（如 Windows 7）。

```
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate /v DisableOSUpgrade /t REG_DWORD /d 1 /f
```

### Windows 11 跳过网络连接和微软账户 {#windows-11-skip-network-connect-and-microsoft-account}

1. 在 OOBE 界面按 Shift + F10 调出命令行窗口
2. 输入命令：`oobe\bypassnro`
