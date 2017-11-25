---
title: 提升效率的必备VSCode插件
categories:
  - 开发工具
tags:
  - VSCode
---


# 常用优质插件的用法

## [TODO+](https://marketplace.visualstudio.com/items?itemName=fabiospampinato.vscode-todo-plus)  

>  创建一个`TODO`列表问题件

**安装**  `ext install vscode-todo-plus` 

**常用命令**

```json
Todo: Open # 打开TODO文件
```

**快捷键**

```json
Cmd/Ctrl+Enter # 将TODO标记为box
Alt+S # 将TODO标记为started
Alt+D # 将TODO标记为done
Alt+C # 将TODO标记为cancel
```

**常用配置**

```json
"todo.symbols.box": "☐"
"todo.symbols.done": "✔"
"todo.symbols.cancel": "✘"
"todo.tags.names": ["critical", "high", "low", "today"]
```

**示例文件**

```yaml
Todo:
  New:
    ☐ 项目列表
    ☐ @critical @high @low @today 高亮标签
    ☐ *加粗* _倾斜_ ~删除~ `代码`
  Done:
   ✔ 完成 @done(17-11-17 22:07)
  Cancel:
   ✘ 取消 @cancelled(17-11-17 22:08)
```



## [Bookmarks](https://marketplace.visualstudio.com/items?itemName=alefragnani.Bookmarks) 

**快捷键**

```json
ctrl + alt + K # Bookmarks: Toggle 
ctrl + alt + L # Jump to Next
ctrl + alt + J # Jump to Previous
```

**常用命令**

```json
Bookmarks: Toggle # 标记/取消 书签标记
Bookmarks: Jump to Next # 下一个书签
Bookmarks: Jump to Previous # 上一个书签
Bookmarks: List # 列出当前文件的所有书签
Bookmarks: List from All Files # 列出所有文件的所有书签
Bookmarks: Clear # 清除当前文件的所有书签
Bookmarks: Clear from All Files # 清除所有文件的所有书签
Bookmarks (Selection): Select Lines # 高亮书签行
Bookmarks (Selection): Expand Selection to Next
Bookmarks (Selection): Expand Selection to Previous
Bookmarks (Selection): Shrink Selection 
```

**常用配置**

```json
"bookmarks.gutterIconPath": "c:\\temp\\othericon.png"
"bookmarks.saveBookmarksInProject": true
"bookmarks.navigateThroughAllFiles": true
"bookmarks.treeview.visible": true
```



## [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) 

> 可以方便的编辑Markdown文件

**快捷键**

```json
Alt + Shift + F # 代码格式化
Ctrl + B # 粗体
Ctrl + I # 斜体
Ctrl + Shift + ] # 标题升级
Ctrl + Shift + [ # 标题降级
Alt + C # 选中/取消 任务列表
```

**常用配置**

```json
"markdown.extension.quickStyling": true
"markdown.extension.toc.orderedList": true
"markdown.extension.preview.autoShowPreviewToSide": true
"markdown.extension.showExplorer": true
```



# 常用插件分类

## 代码高亮与语言支持

### [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)  

> 提供对Vue的支持，资料:  [Vetur Doc - GitBook](https://vuejs.github.io/vetur/)

**Snippet**

```json
scaffold # 展开vue模板
template # html、pug
script # js、ts
style # css、less、scss、sass、postcss、stylus
```

**常用配置**

```json
"eslint.autoFixOnSave": true,
"eslint.validate": [
  {
    "language": "vue",
    "autoFix": true
  }
]
```

**智能感知**

```bash
npm i -S lodash
npm i -D @types/lodash
```

```js
import * as _ from 'lodash'
// 使用 _. 就可以得到 lodash 代码提示
```



### [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight) 

**高亮关键字**

```
TODO: FIXME:
```

**常用命令**

```
Toggle highlight
List hilighted annotations
```

**常用配置**

```json
"todohighlight.isCaseSensitive": false
"todohighlight.include": [
  "**/*.js",
  "**/*.jsx",
  "**/*.ts",
  "**/*.tsx",
  "**/*.html",
  "**/*.php",
  "**/*.css",
  "**/*.scss",
  "**/*.vue"
]
```



### [vue-ls](https://marketplace.visualstudio.com/items?itemName=vaniship.vue-ls)  

> 这个扩展为 VSCode 增加了 vue 语言服务，解决 vue 单文件组件(.vue)文件的语法识别问题。

**.vuelsrc文件配置**

To support file path alias

```js
const path = require('path')

function resolve (dir) {
  return path.join(__dirname, dir)
}

module.exports = {
  resolve: {
    alias: {
      '@': resolve('src')
    }
  }
}
```



- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 




## 代码验证

- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) 提供ECMAScript代码验证


- [TSLint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) 提供TypeScript代码验证
- [stylelint](https://marketplace.visualstudio.com/items?itemName=hollowtree.vue-snippets) css/less/scss验证
- [puglint](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-puglint) 
- [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint) 
- [jshint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.jshint) 
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) 拼写检查




## 代码提示

### [Vue VSCode Snippets](https://marketplace.visualstudio.com/items?itemName=sdras.vue-vscode-snippets) 

**Snippet**

```bash
Vue
  vbase
Template
  vfor | vmodel | vmodel-num | von | vel-props
  vsrc | vstyle | vstyle-obj | vanim | vnuxtl
  vclass | vclass-obj | vclass-obj-mult
Script
  vdata | vmethod | vcomputed | vwatcher | vprops
  vimport | vimport-c | vimport-export | vimport-lib | vimport-gsap
  vfilter | vc-direct | vmixin | vmixin-use
  vanimhook-js | vcommit | vdispatch
  vinc | vdec
Vuex
  vstore | vgetter | vmutation | vaction | vstore-import
Nuxt
  nfont | ncss
```



- [Vue Snippets(vue-ls)](https://marketplace.visualstudio.com/items?itemName=vaniship.vue-ls-snippets) 


- [Vue 2 Snippets](https://marketplace.visualstudio.com/items?itemName=hollowtree.vue-snippets) 
- [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css) 
- [Modern (ES6+) Javascript Snippets](https://marketplace.visualstudio.com/items?itemName=tunnckocore.modern-javascript-snippets) 
- [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css) 




## 智能感知

- [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense) 
- [Less IntelliSense](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-less) 
- [npm Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense) 
- [PHP IntelliSense](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-intellisense) 
- [IntelliSense for CSS class names](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion) 




## 代码美化

### [Pug beautify](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-puglint) 

**常用命令**

```
Beautify pug/jade
```



### [vscode-json](https://marketplace.visualstudio.com/items?itemName=andyyaldoo.vscode-json)

**快捷键**

```json
cmd+alt+v # Validate
cmd+alt+b # Beautify
cmd+alt+u # Uglify
cmd+alt+' # Escape
cmd+alt+; # Unescape
```

**常用命令**

```json
vscode-json: Validate
vscode-json: Beautify
vscode-json: Uglify
vscode-json: Escape
vscode-json: Unescape
```



### [JS-CSS-HTML Formatter](https://marketplace.visualstudio.com/items?itemName=lonefy.vscode-JS-CSS-HTML-formatter) 

**常用命令**

```json
Formatter
Formatter Config
Formatter Create Local Config
```

**配置文件**

```json
{
  "onSave": false,
  "javascript": {
    "indent_size": 2,
    "end_with_newline": true
  },
  "css": {
    "indent_size": 2
  },
  "html": {
    "indent_size": 2,
    "indent_inner_html": true
  }
}
```



### [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify) 

**keybindings.json 配置**

```json
{
  "key": "ctrl+b",
  "command": "HookyQR.beautify",
  "when": "editorFocus"
}
```

**.jsbeautifyrc 配置**

在项目根目录下新建`.jsbeautifyrc`进行配置，使用快捷键`ctrl + B`即可代码格式化。

```json
{
  "indent_size": 2,
  "indent_char": " ",
  "css": {
    "indent_size": 2
  }
}
```

可用的配置映射:

| .jsbeautifyrc setting         | VS Code setting                          |
| ----------------------------- | :--------------------------------------- |
| eol                           | files.eol                                |
| tab_size                      | editor.tabSize                           |
| indent_with_tabs *(inverted)* | editor.insertSpaces                      |
| wrap_line_length              | html.format.wrapLineLength               |
| unformatted                   | html.format.unformatted                  |
| indent_inner_html             | html.format.indentInnerHtml              |
| preserve_newlines             | html.format.preserveNewLines             |
| max_preserve_newlines         | html.format.maxPreserveNewLines          |
| indent_handlebars             | html.format.indentHandlebars             |
| end_with_newline              | html.format.endWithNewline (html)        |
| end_with_newline              | file.insertFinalNewLine (css, js)        |
| extra_liners                  | html.format.extraLiners                  |
| space_after_anon_function     | javascript.format.insertSpaceAfterFunctionKeywordForAnonymousFunctions |
| space_in_paren                | javascript.format.insertSpaceAfterOpeningAndBeforeClosingNonemptyParenthesis |



## 模板预览

- [Pug to HTML](https://marketplace.visualstudio.com/items?itemName=ginie.pug2html) 

**常用命令**

```
pug2html
```

- [Preview](https://marketplace.visualstudio.com/items?itemName=ginie.pug2html) 

> A previewer of Markdown, ReStructured Text, HTML, Jade, Pug, Mermaid files, Image's URI or CSS properties for Visual Studio Code.



## 命令工具

- [npm](https://marketplace.visualstudio.com/items?itemName=fknop.vscode-npm) 
- [yarn](https://marketplace.visualstudio.com/items?itemName=gamunu.vscode-yarn) 
- [vscode-Hexo](https://marketplace.visualstudio.com/items?itemName=codeyu.vscode-hexo) 




## 界面主题

- [Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme) 
- [Easy icon theme](https://marketplace.visualstudio.com/items?itemName=jamesmaj.easy-icons) 
- [vscode-icons](https://marketplace.visualstudio.com/items?itemName=robertohuertasm.vscode-icons) 
- [VSCode Great Icons](https://marketplace.visualstudio.com/items?itemName=ms-vscode.atom-keybindings) 




## 按键映射

- [Sublime Text Keymap](https://marketplace.visualstudio.com/items?itemName=ms-vscode.sublime-keybindings) 
- [Atom Keymap](https://marketplace.visualstudio.com/items?itemName=ms-vscode.atom-keybindings) 
- [Vim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) 




## 其他

- [PHP Debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug) 
- [PHP Extension Pack](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-pack) 
- [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) 
- [Code Outline](https://marketplace.visualstudio.com/items?itemName=patrys.vscode-code-outline) 
- [VSNotes](https://marketplace.visualstudio.com/items?itemName=patricklee.vsnotes) 
- [Auto Close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag) 标签自动闭合
- [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner) 代码运行
- [Project Manager](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager) 