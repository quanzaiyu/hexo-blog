---
title: Pug学习笔记02 - 控制流程
categories:
  - 模板引擎
  - Pug学习笔记
tags:
  - Pug
---



# 分支

## if-else

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

## unless

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

## switch-when-default

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

### 展开写法

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



# 循环

## for-in

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

## for-of

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

## each

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

### 对象迭代

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

### each的空判断

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

## while

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



# 参考资料

- [pug入门指南](https://pugjs.org/zh-cn/api/getting-started.html) 
- Segment Fault : [Pug模板（一）](https://segmentfault.com/a/1190000006198621) 
- Github : [pug](https://github.com/pugjs/pug) 
- [jade-lang](http://jade-lang.com/reference) 
- Segment Fault : [10个最好的 JavaScript 模板引擎](https://segmentfault.com/a/1190000000502743) 
- 博客园 : [pug模板引擎(原jade)](http://www.cnblogs.com/xiaohuochai/p/7222227.html) 

