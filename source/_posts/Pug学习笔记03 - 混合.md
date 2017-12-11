---
title: Pug学习笔记03 - 混合(mixins)
categories:
  - 模板引擎
tags:
  - Pug
---



# 重复的代码块

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



# 传入参数

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



# 块(block)的判断

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



# 属性(Attributes)的使用

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



# 剩余参数

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



# 参考资料

- [pug入门指南](https://pugjs.org/zh-cn/api/getting-started.html) 
- Segment Fault : [Pug模板（一）](https://segmentfault.com/a/1190000006198621) 
- Github : [pug](https://github.com/pugjs/pug) 
- [jade-lang](http://jade-lang.com/reference) 
- Segment Fault : [10个最好的 JavaScript 模板引擎](https://segmentfault.com/a/1190000000502743) 
- 博客园 : [pug模板引擎(原jade)](http://www.cnblogs.com/xiaohuochai/p/7222227.html) 

