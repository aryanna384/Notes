# VSCode个人开发配置

## setting.json

```json
{
    "workbench.iconTheme": "material-icon-theme",
    "editor.fontSize": 15,
    "editor.fontFamily": "Monaco, Consolas, 'Courier New', monospace",
    "editor.fontLigatures": true,
    "git.autofetch": true,
    "editor.formatOnSave": true,
    "javascript.format.insertSpaceAfterFunctionKeywordForAnonymousFunctions": false,
    "vetur.format.defaultFormatter.js": "prettier",
    "prettier.singleQuote": true,
    "prettier.semi": false,
    "vetur.format.defaultFormatter.html": "js-beautify-html",
    "eslint.format.enable": true,
    "files.associations": {
        "*.vue": "vue"
    },
    "editor.suggestSelection": "first",
    "git.enableSmartCommit": true,
    "git.confirmSync": false,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "[vue]": {
        "editor.defaultFormatter": null
    },
    "[javascript]": {
        "editor.defaultFormatter": "dbaeumer.vscode-eslint"
    }
}
```

## VSCode插件安装列表

```
1.Vetur
2.Prettier
3.ESLint
4.Material Icon Theme
5.npm Intellisense
6.Path Intellisense
```

## .prettierrc 配置

```json
{
  "semi": false,
  "singleQuote": true,
  "printWidth": 80
}
```
