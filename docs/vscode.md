<!--
 * @Author: yunfan
 * @Date: 2019-11-22 11:52:28
 * @LastEditTime: 2019-11-22 11:55:51
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Tsign/fed-standard/docs/vscode.md
 -->
# vscode
vscode统一基础配置

## 基本配置
推荐的vscode配置（待议）
```
{
  "editor.tabSize": 2,
  
  "eslint.enable": true,
  "eslint.alwaysShowStatus": true,
  "eslint.packageManager": "yarn",
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "html",
      "autoFix": true
    },
    {
      "language": "vue",
      "autoFix": true
    },
    {
      "language": "typescript",
      "autoFix": true
    },
    {
      "language": "typescriptreact",
      "autoFix": true
    }
  ]
  "editor.formatOnPaste": false,
  "editor.formatOnSave": false,
  "editor.formatOnType": false,
}
```

## 插件 (待议)
- Vetur
- ESlint
- GitLens
- JavaScript (ES6) code snippets
- koroFileHeader 生成头部注释
- TODO highlight
- ...

