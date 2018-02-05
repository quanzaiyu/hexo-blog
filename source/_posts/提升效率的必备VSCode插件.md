---
title: 提升效率的必备VSCode插件
categories:
  - 开发工具
tags:
  - VSCode
---


# 常用优质插件的用法

## [TODO+](https://marketplace.visualstudio.com/items?itemName=fabiospampinato.vscode-todo-plus)  

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/020.png)

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

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/021.png)

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

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/022.gif)

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

### [vscode wxml](https://marketplace.visualstudio.com/items?itemName=coderfee.vscode-wxml)

> 提供对微信小程序 `wxml` 语法高亮的支持。



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

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/016.png)

### [Weex](https://marketplace.visualstudio.com/items?itemName=jaylinwang.weex) 

提供 `vuejs` (.vue)、`weex` (.we) 的语言支持， [Weex官网地址](http://weex.apache.org/cn/guide/) 。



### [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)  

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/023.png)

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

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/024.gif)

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



### [HTML CSS Support ](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css) 

让 html 标签上写class 智能提示当前项目所支持的样式，新版已经支持scss文件检索。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/027.jpg)



- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 




## 代码验证

- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) 提供ECMAScript代码验证


- [TSLint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) 提供TypeScript代码验证


- [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint) 
- [jshint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.jshint) 




### [stylelint](https://marketplace.visualstudio.com/items?itemName=shinnn.stylelint) 

> css/less/scss验证

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/033.png)



### [puglint](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-puglint) 

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/032.png)



### [HTMLHint](https://marketplace.visualstudio.com/items?itemName=mkaufman.HTMLHint) 

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/026.png)



### [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) 

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/025.gif)



## 代码提示与自动完成

- [Vue Snippets(vue-ls)](https://marketplace.visualstudio.com/items?itemName=vaniship.vue-ls-snippets) 


- [Vue 2 Snippets](https://marketplace.visualstudio.com/items?itemName=hollowtree.vue-snippets) 
- [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css) 
- [Modern (ES6+) Javascript Snippets](https://marketplace.visualstudio.com/items?itemName=tunnckocore.modern-javascript-snippets) 
- [Bootstrap 3 Snippets](https://marketplace.visualstudio.com/items?itemName=wcwhitehead.bootstrap-3-snippets) 
- [Bootstrap 4 & Font awesome snippets](https://marketplace.visualstudio.com/items?itemName=thekalinga.bootstrap4-vscode) 




### [vscode-wechat](https://marketplace.visualstudio.com/items?itemName=qinjia.vscode-wechat)



提供小程序开发对应的`wxml`、`wxss`语法高亮支持，并提供对应的 snippets



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



### [VueHelper](https://marketplace.visualstudio.com/items?itemName=oysun.vuehelper) 

**Snippets** and more feature

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/017.gif)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/018.gif)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/019.gif)



### [fileheader](https://marketplace.visualstudio.com/items?itemName=mikey.vscode-fileheader) 

添加文件头，包括用户名、更新日期，使用`Ctrl + Alt + I`进行标记。

配置文件:

```json
{
  "fileheader.Author": "tom",
  "fileheader.LastModifiedBy": "jerry"
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/007.gif)



### [Document This](https://marketplace.visualstudio.com/items?itemName=joelday.docthis) 

为`js`和`ts`添加`JSDoc`风格的注释，使用`Ctrl + Alt + D + D` 激活。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/004.gif)



### [jQuery Code Snippets](https://marketplace.visualstudio.com/items?itemName=donjayamanne.jquerysnippets) 

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/028.png)



### [Easy LESS](https://marketplace.visualstudio.com/items?itemName=mrcrowl.easy-less) 

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/037.gif)



### [:emojisense:](https://marketplace.visualstudio.com/items?itemName=bierner.emojisense) 

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/039.gif)

常用配置:

```json
"emojisense.languages": {
    "markdown": true,
    "git-commit": false,
    "plaintext": false,
    "json": true
}
```



### [Emoji](https://marketplace.visualstudio.com/items?itemName=Perkovec.emoji)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/040.gif)



## 智能感知

- [Less IntelliSense](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-less) 

- [PHP IntelliSense](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-intellisense) 

  ​


### [IntelliSense for CSS class names](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion) 

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/034.gif)



### [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense) 

> 自动路劲补全

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/029.gif)



### [Npm Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense) 

> require 时的包提示（最新版的vscode已经集成此功能）

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/030.gif)



### [Atuo Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag) 

> 自动完成标签名修改匹配

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/001.gif)



## 代码美化

### [Pug beautify](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-pugbeautify)  

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/031.gif)

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

### [Pug to HTML](https://marketplace.visualstudio.com/items?itemName=ginie.pug2html) 

**常用命令**

```
pug2html
```



### [Preview](https://marketplace.visualstudio.com/items?itemName=ginie.pug2html) 

> A previewer of Markdown, ReStructured Text, HTML, Jade, Pug, Mermaid files, Image's URI or CSS properties for Visual Studio Code.



## 命令工具

- [npm](https://marketplace.visualstudio.com/items?itemName=fknop.vscode-npm) 
- [yarn](https://marketplace.visualstudio.com/items?itemName=gamunu.vscode-yarn) 
- [vscode-Hexo](https://marketplace.visualstudio.com/items?itemName=codeyu.vscode-hexo) 




## 界面主题、图标与颜色

- [Material Theme](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-material-theme) [Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme) 
- [Easy icon theme](https://marketplace.visualstudio.com/items?itemName=jamesmaj.easy-icons) 
- [vscode-icons](https://marketplace.visualstudio.com/items?itemName=robertohuertasm.vscode-icons) 
- [VSCode Great Icons](https://marketplace.visualstudio.com/items?itemName=ms-vscode.atom-keybindings) 
- [Dracula](https://marketplace.visualstudio.com/items?itemName=oysun.vuehelper) [官网](https://draculatheme.com/visual-studio-code/) 
- [One Dark Pro](https://marketplace.visualstudio.com/items?itemName=zhuangtongfa.Material-theme) 
- [One Monokai Theme](https://marketplace.visualstudio.com/items?itemName=azemoh.one-monokai) 
- [Darcula](https://marketplace.visualstudio.com/items?itemName=rokoroku.vscode-theme-darcula) 




### [Output Colorizer](https://marketplace.visualstudio.com/items?itemName=IBM.output-colorizer) 

让控制台输出增添色彩。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/010.jpg)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/011.jpg)





## 按键映射

- [Sublime Text Keymap](https://marketplace.visualstudio.com/items?itemName=ms-vscode.sublime-keybindings) 
- [Atom Keymap](https://marketplace.visualstudio.com/items?itemName=ms-vscode.atom-keybindings) 
- [Vim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) 




## 括号匹配

### [Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer) 

让括号拥有独立的颜色，易于区分。可以配合任意主题使用。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/009.png)



### [Subtle Brackets](https://segmentfault.com/a/1190000006697219) 

为括号匹配增加样式。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/012.png)

配置文件:

```json
"subtleBrackets.styles": {
    "global": {
        "borderWidth": "1px",
        "borderStyle": "none none solid none"
    },
    "[]": { 
        "color": "white",
        "backgroundColor": "red",
        "borderStyle": "none"
    }
}
```



### [bracket-jumper](https://marketplace.visualstudio.com/items?itemName=sashaweiss.bracket-jumper) 

在匹配的括号之间跳转。

快捷键:

```
bracket-jumper.jumpLeft:    { Mac: ctrl+left, Windows/Linux: ctrl+alt+left }
bracket-jumper.jumpRight:   { Mac: ctrl+right, Windows/Linux: ctrl+alt+right }
bracket-jumper.selectLeft:  { Mac: ctrl+shift+left, Windows/Linux: ctrl+alt+shift+left }
bracket-jumper.selectRight: { Mac: ctrl+shift+right, Windows/Linux: ctrl+alt+shift+right }
bracket-jumper.ascendLeft:        { Mac: ctrl+up, Windows/Linux: ctrl+alt+up }
bracket-jumper.ascendRight:       { Mac: ctrl+down, Windows/Linux: ctrl+alt+down }
bracket-jumper.selectAscendLeft:  { Mac: ctrl+shift+up, Windows/Linux: ctrl+alt+shift+up }
bracket-jumper.selectAscendRight: { Mac: ctrl+shift+down, Windows/Linux: ctrl+alt+shift+down }
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/013.gif)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/014.gif)



### [Bracket Selection](https://marketplace.visualstudio.com/items?itemName=guosong.bracketselection) 

选择匹配括号之间的内容。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/015.gif)



## 版本控制

### [Git Lens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) 

丰富的git日志插件。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/005.gif)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/006.png)



## 项目管理

### [Project Manager](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager) 

在多个项目之前快速切换的工具。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/003.png)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/002.gif)



## 其他

- [PHP Debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug) 
- [PHP Extension Pack](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-pack) 
- [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) 
- [Code Outline](https://marketplace.visualstudio.com/items?itemName=patrys.vscode-code-outline) 
- [VSNotes](https://marketplace.visualstudio.com/items?itemName=patricklee.vsnotes) 
- [Auto Close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag) 标签自动闭合
- [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner) 代码运行




### [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner)

To run code: 

- use shortcut `Ctrl+Alt+N`
- or press `F1` and then select/type `Run Code`,
- or right click the Text Editor and then click `Run Code` in editor context menu
- or click `Run Code` button in edit
- or title menuor click `Run Code` button in context menu of file explorer

To stop the running code: 

- use shortcut `Ctrl+Alt+M`
- or press `F1` and then select/type `Stop Code Run`
- or right click the Output Channel and then click `Stop Code Run` in context menu




### [Document This](https://marketplace.visualstudio.com/items?itemName=joelday.docthis) 

为`js`和`ts`添加`JSDoc`风格的注释，使用`Ctrl + Alt + D + D` 激活。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/004.gif)



### [filesize](https://marketplace.visualstudio.com/items?itemName=zh9528.file-size) 

在状态栏显示文件大小，点击后还可以看到详细创建、修改时间。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/008.png)



### [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) 

让 vscode 映射 chrome 的 debug功能，静态页面都可以用 vscode 来打断点调试。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/038.png)



### [px2rem](https://marketplace.visualstudio.com/items?itemName=arturiapendragon.px2rem) 

一个CSS值转REM的VSCode插件

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/035.gif)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/vscode/036.gif)



# 参考资料

[vscode 插件推荐 - 献给所有前端工程师（2017.8.18更新）](https://segmentfault.com/a/1190000006697219) 