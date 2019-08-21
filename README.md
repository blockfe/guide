# guide

> BlockFE Guidelines

## 为什么需要编码规范？

一个主要原因是：每个人都以不同的方式编写代码。我可能喜欢以某种方式做某件事，而且你可能喜欢以不同的方式去做。如果我们每个人都只在我们自己的代码上工作，这样并没有什么问题。但是，如果你有一个 10个，100个甚至1000个开发人员的团队，都在同一个代码库上工作，会发生什么呢？事情变得非常糟糕。 编码规范可以使新开发人员快速掌握代码，然后编写出其他开发人员可以快速轻松理解的代码！

**使用到的工具：**

- [editorConfig 编辑器配置文件](#editorConfig)
- [Prettier 批量格式化代码](#Prettier)
- [Eslint js、vue 文件代码检查规范](#Eslint)
- [Stylelint css 文件检查](#Stylelint)
- [lint-staged 提交到git之前跑一次代码检查](#lint-staged)
- [VSCode 配置和插件](#VSCode)

## editorConfig

当你在编码时，EditorConfig 插件会去查找当前编辑文件的所在文件夹或其上级文件夹中是否有 `.editorconfig` 文件。如果有，则编辑器的行为会与 `.editorconfig` 文件中定义的一致，并且其优先级高于编辑器自身的设置。

最后附上我的 `.editorconfig` 文件：

```javascript
root = true

charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
```

## Prettier

`ESLint` 可以检测出你代码中潜在的问题，比如使用了某个变量却忘记了定义，而 `Prettier` 作为代码格式化工具，能够统一你或者你的团队的代码风格。

现在，JavaScript终于有了一个[解决方案](http://jlongster.com/A-Prettier-Formatter)。有一个新工具，叫做[`Prettier`](https://github.com/prettier/prettier)，它**运用自身的规则将你的的代码重新格式化**。无论你之前的代码风格是怎样。

Prettier 已经被一些非常流行的项目比如 `React` 和 `Babel` 采用了。对于我自己的项目，我已经开始从自己的个性化风格全部转为 `Prettier` 风格。相比于Airbnb代码风格，我更推荐Prettier。

Prettier可以在保存文件的时候可以自动统一风格。

项目级的配置，在项目根目录添加配置文件`.prettierrc`：

```javascript
{
  "eslintIntegration": true,
  "stylelintIntegration": true,
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "none",
  "bracketSpacing": true,
  "jsxBracketSameLine": true,
  "arrowParens": "always"
}
```

## Eslint

在 ESLint 中使用 eslint-plugin-prettier，**为什么使用这种方法: 希望格式化结果完全符合 Prettier 的要求。**

相关依赖:

```bash
npm install -D prettier eslint-config-prettier eslint-plugin-prettier
```

`eslint-plugin-prettier` 的工作原理是，对比格式化前和用 Prettier 格式化后的代码，有不一致的地方就会报错，然后你可以手动运行 `eslint --fix` 来修复。

不过 Prettier 的格式化很可能和你使用的 ESLint 配置冲突，比如你的 ESLint 设定的不使用分号，而 Prettier 默认使用分号，这时候你需要配置 Prettier 让它不使用分号。反之如果你想使用 Prettier 的规则而不是 ESLint 的，为防止 ESLint 报错，你需要使用 `eslint-config-prettier` 来关闭所有可能引起冲突的规则。

Vue-cli3初始化的项目配置如下 `package.json`：

```javascript
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true
  },
  extends: [
    'plugin:prettier/recommended',
    'plugin:vue/essential',
    'eslint:recommended'
  ],
  parserOptions: {
    parser: 'babel-eslint'
  },
  rules: {}
};
```

## Stylelint

stylelint 是『一个强大的、现代化的 CSS 检测工具』, 与 `ESLint` 类似, 是通过定义一系列的编码风格规则帮助我们避免在样式表中出现错误.

`stylelint` 出现前已经有 `csslint` 工具, 与 `ESLint` 出现前的 `JSLint` 类似, 由于技术局限它们的语法校验规则很难扩展, 继而由新的工具替代。`stylelint` 出现在 postcss 诞生之后, 因而它可以『理解』类 CSS 语法, 例如: `CSSNext, PreCSS, LESS, SCSS, SugarSS`。

项目使用的 `.stylelintrc` 配置文件：

```javascript
{
  "extends": "stylelint-config-standard",
  "rules": {
    "no-invalid-double-slash-comments": null,
    "rule-empty-line-before": "never",
    "selector-pseudo-class-no-unknown": [
      true,
      {
        "ignorePseudoClasses": ":global,:local"
      }
    ],
    "selector-pseudo-element-colon-notation": "single",
    "font-family-no-missing-generic-family-keyword": null
  }
}
```

## lint-staged

在提交代码时格式化，安装需要的依赖:

```bash
npm install -D lint-staged husky
```

[husky](https://github.com/typicode/husky) 被用来添加一些 git 钩子，这里我们需要一个用 `pre-commit` 在每次 `git commit` 操作时执行 `lint-staged` 命令。

而 [lint-staged](https://github.com/okonet/lint-staged) 可以对 git 暂存区文件(也就是你想要 commit 的文件)执行一些操作，比如这里我们想在代码被修改后将其格式化:

```javascript
{
  "lint-staged": {
    "*.js": ["eslint --fix", "git add"]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
}
```

## Commitlint

Commitlint被用来校验每次 `git commit` 操作时的 `commit message` 是否按照格式规范书写，安装需要的依赖:

```bash
npm install -D @commitlint/config-angular @commitlint/cli
```

这里我们需要一个用 `commit-msg` 在每次 `git commit` 操作时执行 `commitlint -E HUSKY_GIT_PARAMS` 命令：

```javascript
{
  "lint-staged": {
    "*.js": ["eslint --fix", "git add"]
  },
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS",
      "pre-commit": "lint-staged"
    }
  }
}
```

创建commitlint.config.js文件到项目根目录，设定规范：

```javascript
module.exports = {
  extends: ['@commitlint/config-angular'],
  rules: {
    'subject-case': [0, 'never'],
    'type-enum': [
      2, //代表必须输入
      'always',
      [
        'docs', // Adds or alters documentation. 仅仅修改了文档，比如README, CHANGELOG, CONTRIBUTE等等
        'chore', // Other changes that don't modify src or test files. 改变构建流程、或者增加依赖库、工具等
        'feat', // Adds a new feature. 新增feature
        'fix', // Solves a bug. 修复bug
        'merge', // Merge branch ? of ?.
        'perf', // Improves performance. 优化相关，比如提升性能、体验
        'refactor', // Rewrites code without feature, performance or bug changes. 代码重构，没有加新功能或者修复bug
        'revert', // Reverts a previous commit. 回滚到上一个版本
        'style', // Improves formatting, white-space. 仅仅修改了空格、格式缩进、都好等等，不改变代码逻辑
        'test' // Adds or modifies tests. 测试用例，包括单元测试、集成测试等
      ]
    ]
  }
};
```

## VSCode

推荐使用VSCode，统一Code格式化配置，避免团队小伙伴之间因IDE配置(EditorConfig、Prettier、Vetur...)不同而造成的Commit污染，且保存文件时自动进行ESlint校验，及时发现问题并处理修复。

### 一、自动：使用Settings Sync插件同步配置和插件

1. VSCode安装Settings Sync
2. 配置Sync:Gist为4d5da52c0c4cff63bd677d62bbbf4433
3. Shift+Command+P -> Sync: Download Settings

### 二、手动：安装Plugins和修改Setting.json

#### VSCode Plugins

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
- ......

#### VSCode Setting.json

Shift+Command+P -> Open Settings(JSON)

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
    "vetur.validation.template": false,
    "prettier.tabWidth": 2,
    "prettier.semi": true,
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
    "sync.gist": "4d5da52c0c4cff63bd677d62bbbf4433",
    "files.associations": {
        "*.css": "postcss"
    },
    "window.zoomLevel": 0,
    "emmet.includeLanguages": {
        "vue-html": "html",
        "javascript": "javascriptreact",
        "postcss": "css"
    },
}
```

## 参考

- [Coding Guide](https://guide.aotu.io/)
- [Vue Guide](https://cn.vuejs.org/v2/style-guide/)
- [Git Commit Message](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)

