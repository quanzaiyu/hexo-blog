---
title: Pug学习笔记04 - 文件包含与继承
categories:
  - 模板引擎
tags:
  - Pug
---



# 文件包含(include)

包含（include）功能允许把另外的文件内容插入进来

## 简单使用

index.pug

```jade
doctype html
html
  include includes/head
  body
    h1 我的网站
    p 欢迎来到我这简陋得不能再简陋的网站。
    include includes/foot
```

includes/head.pug

```jade
head
  title 我的网站
```

includes/foot.pug

```jade
footer#footer
  p Copyright (c) foobar
```

index.pug的渲染结果 - index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <title>我的网站</title>
  </head>
  <body>
    <h1>我的网站</h1>
    <p>欢迎来到我这简陋得不能再简陋的网站。</p>
    <footer id="footer">
      <p>Copyright (c) foobar</p>
    </footer>
  </body>
</html>
```



## 包含非pug文件

> 被包含的如果不是 Pug 文件，那么就只会当作文本内容来引入

index.pug

```jade
doctype html
html
  head
    style
      include style.css
  body
    script
      include script.js
```

style.css

```css
/* style */
* {
  margin: 0;
  padding: 0;
}
```

script.js

```js
console.log(1)
```

index.pug的渲染结果 - index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <style>/* style */
* {
  margin: 0;
  padding: 0;
}
    </style>
  </head>
  <body>
    <script>console.log(1)</script>
  </body>
</html>
```



# 文件继承( block 和 extends )

- 使用block导出块
- 使用extends继承模板页
- 在继承页面中声明block替代模板页同名的block
- 如果存在同名block默认会替换相同的block，在模板中声明而在继承页中未声明的block会保留模板页中的布局
- 整个块的继承是递归的

## 简单使用

layout.pug

```jade
include variables.pug
html
  head
    meta(charset="UTF-8")
    title 我的站点 - #{title}
    block scripts
      script(src='/script-layout.js')
  body
    block content
    block foot
      #footer
        p 一些页脚的内容
```

variables.pug

```jade
-var title = "小昱博客"
```

page1.pug

```jade
extends layout.pug

block scripts
  script(src='/script.js')

block content
  h1= title
  - var pets = ['猫', '狗']
  each petName in pets
    include pet.pug
  block sidebar
```

page2.pug

```jade
extends page1

block sidebar
  #sidebar 这是一个侧边栏
```

pet.pug

```jade
p= petName
```

page1.pug 最终渲染为 - page1.html

page1中继承layout并导出块sidebar

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>我的站点 - 小昱博客</title>
    <script src="/script.js"></script>
  </head>
  <body>
    <h1>小昱博客</h1>
    <p>猫</p>
    <p>狗</p>
    <div id="footer">
      <p>一些页脚的内容</p>
    </div>
  </body>
</html>
```

page2.pug 最终渲染为 - page2.html

page2中继承page1

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>我的站点 - 小昱博客</title>
    <script src="/script.js"></script>
  </head>
  <body>
    <h1>小昱博客</h1>
    <p>猫</p>
    <p>狗</p>
    <div id="sidebar">这是一个侧边栏</div>
    <div id="footer">
      <p>一些页脚的内容</p>
    </div>
  </body>
</html>
```



## 扩展模式

- block 默认将会替换模板的内容
- block prepend（向头部添加内容）
- block append（向尾部添加内容）

layout.pug

```jade
html
  head
    block head
      script(src='test1.js')
      script(src='test2.js')
  body
    block content
      p some text
```

page.pug

```jade
extends layout.pug

block prepend head
  title 小昱博客

block append content
  - var pets = ['猫', '狗']
  each petName in pets
    include pet.pug
```

page.pug 最终渲染为 - page.html

```html
<!DOCTYPE html>
<html>
  <head>
    <title>小昱博客</title>
    <script src="test1.js"></script>
    <script src="test2.js"></script>
  </head>
  <body>
    <p>some text</p>
    <p>猫</p>
    <p>狗</p>
  </body>
</html>
```


# 参考资料

- [pug入门指南](https://pugjs.org/zh-cn/api/getting-started.html) 
- Segment Fault : [Pug模板（一）](https://segmentfault.com/a/1190000006198621) 
- Github : [pug](https://github.com/pugjs/pug) 
- [jade-lang](http://jade-lang.com/reference) 
- Segment Fault : [10个最好的 JavaScript 模板引擎](https://segmentfault.com/a/1190000000502743) 
- 博客园 : [pug模板引擎(原jade)](http://www.cnblogs.com/xiaohuochai/p/7222227.html) 

