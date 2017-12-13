---
title: Pug学习笔记01 - 基础
categories:
  - 模板引擎
  - Pug学习笔记
tags:
  - Pug
---



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



# 参考资料

- [pug入门指南](https://pugjs.org/zh-cn/api/getting-started.html) 
- Segment Fault : [Pug模板（一）](https://segmentfault.com/a/1190000006198621) 
- Github : [pug](https://github.com/pugjs/pug) 
- [jade-lang](http://jade-lang.com/reference) 
- Segment Fault : [10个最好的 JavaScript 模板引擎](https://segmentfault.com/a/1190000000502743) 
- 博客园 : [pug模板引擎(原jade)](http://www.cnblogs.com/xiaohuochai/p/7222227.html) 

