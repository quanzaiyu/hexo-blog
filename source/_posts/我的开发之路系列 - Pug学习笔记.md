---
title: Pug学习笔记
categories:
  - 模板引擎
tags:
  - Pug
---



> 本文已发布到我的书籍，具体内容请访问 http://book.node.xiaoyulive.top/497568



# pug简介

Pug原名不叫Pug，原来是大名鼎鼎的jade，后来由于商标的原因，改为Pug，哈巴狗。

# pug的使用

安装

```bash
npm install -g pug-cli
pug -V
```

编译

```bash
pug index.pug # 直接生成html(未格式化)
pug -P index.pug # 生成格式化后的html
pug -P -w index.pug # 监视文件变化并实时输出格式化后的html
```

# pug参数来源

## 来自文件内部

```bash
-var a = 0
p=a
```

## 来自命令行参数

使用`--obj`参数，就可以跟随一个对象形式的参数

```bash
pug -P -w index.pug --obj '{val: "命令行参数"}''
```

## 外部JSON文件

使用`-O`，跟随一个JSON文件的路径即可

```bash
pug -P -w index.pug -O test.json
```

## 优先级

- 这三种方式，pug文件内部的变量优先级最高，而外部JSON文件和命令行传参优先级相同

- 如果外部JSON文件和命令行传参两种方式都存在，由于--obj写在-O后面，最终以后面的参数为准

```bash
pug -P -w index.pug -O test.json --obj '{val: "命令行参数"}''
```



# Pug基础语法

## 基本结构

- pug使用缩进风格
- 属性写在`()`中，多个属性使用`,`分割。
- 内容写在同一行，使用空格分割。
- 子元素换行并缩进。
- 单行注释使用`//`
- 多行注释使用换行缩进的风格
- 不被编译的注释使用`//-`
- 强制换行使用`= '\n'`
- 为了节省空间可以使用`:`嵌套子元素

pug

```jade
//- 不被编译的注释
// muti-line comment
  多行注释
  注意缩进
= '\n'
doctype html
html
  head
    // 单行注释
    meta(charset="UTF-8")
    meta(
      name="viewport", content="width=device-width, initial-scale=1.0")
    meta(http-equiv="X-UA-Compatible", content="ie=edge")
    title Document
  body
    div hello pug
      div: a(href="")
      img(src="", alt="")
    // 属性与内容测试
    h1#title Welcome to 
      a.button(href='www.xiaoyulive.top',title="xiaoyulive") 小昱个人博客
```

html

```html
<!-- muti-line comment
多行注释
注意缩进
-->
<!DOCTYPE html>
<html>
  <head>
    <!-- 单行注释-->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
  </head>
  <body>
    <div>hello pug
      <div><a href=""></a></div><img src="" alt="">
    </div>
    <!-- 属性与内容测试-->
    <h1 id="title">Welcome to <a class="button" href="www.xiaoyulive.top" title="xiaoyulive">小昱个人博客</a></h1>
  </body>
</html>
```

### 条件注释

- 同html条件注释的写法

```html
<!--[if IE 8]>
<html lang="en" class="lt-ie9">
<![endif]-->
<!--[if gt IE 8]><!-->
<html lang="en">
<!--<![endif]-->
```



## 变量声明与使用

- 使用`-var`声明一个变量
- 使用`ele #{var}`或`ele=`使用变量(不转义)
- 使用`ele !{var}`或`ele!=`使用变量(转义)
- 使用`ele \#{var}`或`ele \!{var}`使用变量(编译前)
- 使用没有的变量赋值，输出为空

pug

```jade
-var Pug="hello pug"
//- 使用变量
p #{Pug}

-var htmlData='<strong>sdf</strong>'
//- 转义使用变量
p !{htmlData}
p!=htmlData
//- 不转义使用变量
p #{htmlData}
p=htmlData
//- 编译前的代码
p \#{htmlData}
p \!{htmlData}

//- 没有的变量赋值，输出为空
p=data
```

html

```html
<p>hello pug</p>
<p><strong>sdf</strong></p>
<p><strong>sdf</strong></p>
<p>&lt;strong&gt;sdf&lt;/strong&gt;</p>
<p>&lt;strong&gt;sdf&lt;/strong&gt;</p>
<p>#{htmlData}</p>
<p>#{htmlData}</p>
<p></p>
```



## 长文本

- 使用`.`开启多行文本
- 每一行使用`|` 开启多行文本
- 使用`.`开启的多行文本可以使用html内联标签
- 使用`|`开启的多行文本可以使用pug风格标签
- 脚本一般使用`.`进行分割，在里面可以书写js代码

pug

```jade
p.
  这里是多行文本测试<strong>hi</strong>
  这里是多行文本测试
p
  | 这里是多行文本测试
  strong hi
  | 这里是多行文本测试
script.
  if (usingPug)
    console.log('这是明智的选择。')
  else
    console.log('请用 Pug。')
```

html

```html
<p>
  这里是多行文本测试<strong>hi</strong>
  这里是多行文本测试
</p>
<p>这里是多行文本测试<strong>hi</strong>这里是多行文本测试</p>
<script>
  if (usingPug)
    console.log('这是明智的选择。')
  else
    console.log('请用 Pug。')
</script>
```

### 长文本的标签嵌入

- 长文本可以使用`#[ele content]`的形式嵌入标签

pug

```jade
p.
  这是一个很长很长而且还很无聊的段落，还没有结束，是的，非常非常地长。
  突然出现了一个 #[strong 充满力量感的单词]，这确实让人难以 #[em 忽视]。
```

html

```html
<p>
  这是一个很长很长而且还很无聊的段落，还没有结束，是的，非常非常地长。
  突然出现了一个 <strong>充满力量感的单词</strong>，这确实让人难以 <em>忽视</em>。
</p>
```

# 控制流程

## 分支

### if-else

pug

```jade
-var a = 5
if a == 1
  p 1
else if a == 2
  p 2
else
  p false
```

html

```html
<p>false</p>
```

### unless

unless语句当表达式为false的时候执行

pug

```jade
-var istrue=false
unless istrue
  p hello
```

html

```html
<p>hello</p>
```

### switch-when-default

pug

```jade
-var name='pug'
case name
  when 'java': p Hi,java
  when 'pug': p Hi,pug
  default: p Hi
```

html

```html
<p>Hi,pug</p>
```

#### 展开写法

pug

```jade
-var name='pug'
case name
  when 'java'
    p Hi,java
  when 'pug'
    p Hi,pug
  default
    p Hi
```

html

```html
<p>Hi,pug</p>
```



## 循环

### for-in

pug

```jade
-var array=[100,200,300,400]
-for (var k in array)
  p=array[k] + k
```

html

```html
<p>1000</p>
<p>2001</p>
<p>3002</p>
<p>4003</p>
```

### for-of

pug

```jade
-var array=[1,2,3,4]
-for (var v of array)
  p=v
```

html

```html
<p>1</p>
<p>2</p>
<p>3</p>
<p>4</p>
```

### each

pug

```jade
each val in [1, 2, 3, 4, 5]
  li= val

each val, key in [1, 2, 3, 4, 5]
  li= `${key} - ${val}`
```

html

```html
<li>1</li>
<li>2</li>
<li>3</li>
<li>4</li>
<li>5</li>
<li>0 - 1</li>
<li>1 - 2</li>
<li>2 - 3</li>
<li>3 - 4</li>
<li>4 - 5</li>
```

#### 对象迭代

pug

```jade
ul
  each val, index in {1:'一',2:'二',3:'三'}
    li= index + ': ' + val
```

html

```html
<ul>
  <li>1: 一</li>
  <li>2: 二</li>
  <li>3: 三</li>
</ul>
```

#### each的空判断

方法一

pug

```jade
- var values = []
ul
  each val in values.length ? values : ['没有内容']
    li= val
```

html

```html
<ul>
  <li>没有内容</li>
</ul>
```

方法二: each-else

pug

```jade
- var values = []
ul
  each val in values
    li= val
  else
    li 没有内容
```

html

```html
<ul>
  <li>没有内容</li>
</ul>
```

### while

pug

```jade
- var n = 0;
ul
  while n < 4
    li= n++
```

html

```html
<ul>
  <li>0</li>
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
```



# 混合

## 重复的代码块

pug

```jade
mixin sayHi
  p hello everyone
+sayHi
+sayHi
```

html

```html
<p>hello everyone</p>
<p>hello everyone</p>
```



## 传入参数

pug

```jade
mixin person(name, age)
  li.name= '我的名字叫' + name
  li.age= '今年' + age + '岁了'
ul
  +person('cat', 18)
```

html

```html
<ul>
  <li class="name">我的名字叫cat</li>
  <li class="age">今年18岁了</li>
</ul>
```



## 块(block)的判断

pug

```jade
mixin article(title)
  .article
    h1= title
    //- 判断是否存在块
    if block
      block
    else
      p No content provided
+article('Hello world')
  //- 不存在块
+article('Hello world')
  //- 存在块
  p This is my
```

html

```html
<div class="article">
  <h1>Hello world</h1>
  <p>No content provided</p>
</div>
<div class="article">
  <h1>Hello world</h1>
  <p>This is my</p>
</div>
```



## 属性(Attributes)的使用

混入也可以隐式地从“标签属性”得到一个参数 attributes

pug

```jade
mixin link(href, name)
  div
    a(
      class=attributes.class,
      id=attributes.id,
      href=href)= name
+link('/foo', 'foo')(class="btn", id='a')
```

html

```html
<div><a class="btn" id="a" href="/foo">foo</a></div>
```

> 注意: +link(class="btn") 等价于 +link()(class="btn")，因为 Pug 会判断括号内的内容是属性还是参数。但最好使用后面的写法，明确地传递空的参数，确保第一对括号内始终都是参数列表。



## 剩余参数

pug

```jade
mixin list(id, ...items)
  ul(id=id)
    each item in items
      li= item
+list('my-list', 1, 2, 3, 4)
```

html

```html
<ul id="my-list">
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
</ul>
```



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
- Segment Fault : [10个最好的 JavaScript 模板引擎](https://segmentfault.com/a/1190000000502743) 
- Github : [pug](https://github.com/pugjs/pug) 
- [jade-lang](http://jade-lang.com/reference) 
- 博客园 : [pug模板引擎(原jade)](http://www.cnblogs.com/xiaohuochai/p/7222227.html) 

