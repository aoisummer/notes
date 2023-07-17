---
tags:
  - vscode
---

# VSCode 配置

## 工作区推荐扩展 {#workspace-recommend-extensions}

这里推荐了 eslint 和 prettier 两个扩展。

```json title=".vscode/extensions.json"
{
  "recommendations": ["dbaeumer.vscode-eslint", "esbenp.prettier-vscode"]
}
```

## 工作区选项设置 {#workspace-settings}

这里启用了如下配置：

- 保存时进行格式化
- 保存时对 import 进行排序

```json title=".vscode/settings.json"
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.sortImports": true
  }
}
```

:::note

对于当前用户的全局配置，其文件位置位于 `%APPDATA%\Code\User\settings.json`

:::
