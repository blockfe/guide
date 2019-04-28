# guide

> BlockFE Guidelines

## Guide Docs

- [Coding Guide](https://guide.aotu.io/)
- [Vue Guide](https://cn.vuejs.org/v2/style-guide/)
- [Git Commit Message](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)

## Guide IDE

推荐使用VSCode，统一Code格式化配置，避免团队小伙伴之间因IDE配置(EditorConfig、Prettier、Vetur...)不同而造成的Commit污染，且保存文件时自动进行ESlint校验，及时发现问题并处理修复。
(安装：Settings Sync插件可同步VSCode配置)

### VSCode Plugins

- **One Dark Pro** 主题
- **VSCode Great Icons** 图标主题
- **FiraCode** 字体[https://github.com/tonsky/FiraCode]
- **Bracket Pair Colorizer** 每一对括号用不同颜色区别 （括号强迫症必备）
- **Code Runner** node，python等代码不必开命令行即可运行
- **Code Spell Checker** 单词拼写检测
- **EditorConfig for VS Code** 支持引入EditorConfig配置
- **Guides** 在编辑器中显示更完善的参考线
- **Eslint** 语法检测
- **Git History** git提交历史
- **GitLens** 在代码中显示每一行代码的提交历史
- **HTML CSS Support** VSCode对html，css文件支持，便于你快速书写属性
- **HTML Snippets** 完整的HTML标记，包括HTML5代码段
- **JavaScript (ES6) code snippets** ES6语法中的JavaScript代码片段（支持JavaScript和TypeScript）。
- **Auto Close Tag** 自动补充html/xml close tag
- **Color Highlight** 在编辑器中显示颜色色块
- **Path Intellisense** 路径识别苦战，比如书写图片路径时。遗憾就是，对webpack项目中的路径别名无法扩展
- **Import Cost** 在编辑器中显示导入/需要包大小
- **Prettier** 格式化，使用标准风格，快捷键 alt+shift+F
- **Vetur** 添加对.vue后缀文件的快速书写支持。
- **Settings Sync** 用于同步VSCode配置，多台电脑一份配置
- **markdownlint** 书写md文件的预览插件

### VSCode setting.json

```javascript
{
    // editor 是针对vscode的风格设置
    "editor.tabSize": 2,
    "editor.matchBrackets": false,
    "editor.renderIndentGuides": false,
    "editor.fontFamily": "Fira Code",
    "editor.fontSize": 14,
    "editor.fontLigatures": true,
    "editor.multiCursorModifier": "ctrlCmd",
    "editor.snippetSuggestions": "top",
    "editor.quickSuggestions": {
        "other": true,
        "comments": true,
        "strings": false
    },
    // workbench 是针对vscode的主题设置
    "workbench.iconTheme": "vscode-great-icons",
    "workbench.editor.enablePreview": false,
    "workbench.colorTheme": "One Dark Pro",
    "workbench.colorCustomizations": {
        // 设置guide线高亮颜色
        "editorIndentGuide.activeBackground": "#ff0000"
    },
    // vscode 文件搜索区域配置
    "search.exclude": {
        "**/dist": true,
        "**/build": true,
        "**/elehukouben": true,
        "**/.git": true,
        "**/.gitignore": true,
        "**/.svn": true,
        "**/.DS_Store": true,
        "**/.idea": true,
        "**/.vscode": false,
        "**/yarn.lock": true,
        "**/tmp": true
    },
    /** Necessary configuration - START **/
    // vscode 默认格式化插件
    "[html]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[json]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[jsonc]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[css]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[scss]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[less]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    // vetur 是用于vue开发时的代码格式化
    "vetur.format.defaultFormatter.html": "js-beautify-html",
    "vetur.format.defaultFormatterOptions": {
        "js-beautify-html": {
            "wrap_attributes": "aligned-multiple"
        }
    },
    "vetur.format.defaultFormatter.js": "vscode-typescript",
    // prettier 格式化配置
    "prettier.tabWidth": 2,
    "prettier.semi": true,
    "prettier.printWidth": 200,
    "prettier.proseWrap": "preserve",
    "prettier.singleQuote": true,
    "prettier.trailingComma": "none",
    "prettier.bracketSpacing": true,
    "prettier.jsxBracketSameLine": true,
    "prettier.arrowParens": "always",
    // 保存时自动格式化
    "editor.formatOnPaste": true,
    // 是否开启eslint检测
    "eslint.enable": true,
    // 文件保存时，是否自动根据eslint进行格式化
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
        "typescript",
        "typescriptreact"
    ],
    /** Necessary configuration - END **/
    // git是否启用自动拉取
    "git.autofetch": false,
    // 配置gitlen中git提交历史记录的信息显示情况
    "gitlens.advanced.messages": {
        "suppressCommitHasNoPreviousCommitWarning": false,
        "suppressCommitNotFoundWarning": false,
        "suppressFileNotUnderSourceControlWarning": false,
        "suppressGitVersionWarning": false,
        "suppressLineUncommittedWarning": false,
        "suppressNoRepositoryWarning": false,
        "suppressResultsExplorerNotice": false,
        "suppressShowKeyBindingsNotice": true,
        "suppressUpdateNotice": true,
        "suppressWelcomeNotice": false
    },
    // git配置sync同步
    "sync.gist": "4d5da52c0c4cff63bd677d62bbbf4433"
}
```

### .editorconfig

```javascript
root = true

charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
```