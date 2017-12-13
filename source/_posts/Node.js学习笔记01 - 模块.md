---
title: Node.js学习笔记01 - 模块
categories:
  - Node.js
  - Node.js学习笔记
tags:
  - Node.js
---



# CommonJS

在`CommonJS`这个规范下，每个`.js`文件都是一个模块，它们内部各自使用的变量名和函数名都互不冲突，例如，`hello.js`和`main.js`都申明了全局变量`var s = 'xxx'`，但互不影响。

一个模块想要对外暴露变量（函数也是变量），可以用`module.exports = variable;`，一个模块要引用其他模块暴露的变量，用`var ref = require('module_name');`就拿到了引用模块的变量。

要在模块中对外输出变量，用：

```js
module.exports = variable;
```

输出的变量可以是任意对象、函数、数组等等。

要引入其他模块输出的对象，用：

```js
var foo = require('other_module');
```

引入的对象具体是什么，取决于引入模块输出的对象。



## module.exports 和 exports

在Node环境中，有两种方法可以在一个模块中输出变量：

方法一：对module.exports赋值：

```js
// hello.js

function hello() {
    console.log('Hello, world!');
}

function greet(name) {
    console.log('Hello, ' + name + '!');
}

module.exports = {
    hello: hello,
    greet: greet
};
```

方法二：直接使用exports：

```js
// hello.js
function hello() {
    console.log('Hello, world!');
}

function greet(name) {
    console.log('Hello, ' + name + '!');
}

function hello() {
    console.log('Hello, world!');
}

exports.hello = hello;
exports.greet = greet;
```

默认情况下，Node准备的`exports`变量和`module.exports`变量实际上是同一个变量，并且初始化为空对象`{}`，于是，我们可以写：

```js
exports.foo = function () { return 'foo'; };
exports.bar = function () { return 'bar'; };
```

也可以写：

```js
module.exports.foo = function () { return 'foo'; };
module.exports.bar = function () { return 'bar'; };
```

换句话说，Node默认给你准备了一个空对象`{}`，就可以直接往里面加东西。

但是，如果我们要输出的是一个函数或数组，那么，只能给`module.exports`赋值：

```js
module.exports = function () { return 'foo'; };
```

给`exports`赋值是无效的，因为赋值后，`module.exports`仍然是空对象`{}`。



# 基本模块

## global

JavaScript有且仅有一个全局对象，在浏览器中，叫`window`对象。而在Node.js环境中，也有唯一的全局对象，但不叫`window`，而叫`global`，这个对象的属性和方法也和浏览器环境的`window`不同。

```js
> global.console
Console {
  log: [Function: bound consoleCall],
  info: [Function: bound consoleCall],
  warn: [Function: bound consoleCall],
  error: [Function: bound consoleCall],
  dir: [Function: bound consoleCall],
  time: [Function: bound consoleCall],
  timeEnd: [Function: bound consoleCall],
  trace: [Function: bound consoleCall],
  assert: [Function: bound consoleCall],
  Console: [Function: Console],
  debug: [Function: debug],
  dirxml: [Function: dirxml],
  table: [Function: table],
  group: [Function: group],
  groupCollapsed: [Function: groupCollapsed],
  groupEnd: [Function: groupEnd],
  clear: [Function: clear],
  count: [Function: count],
  markTimeline: [Function: markTimeline],
  profile: [Function: profile],
  profileEnd: [Function: profileEnd],
  timeline: [Function: timeline],
  timelineEnd: [Function: timelineEnd],
  timeStamp: [Function: timeStamp] }
```



## process

`process` 代表当前Node.js进程。

```js
process.version # 当前Node版本
process.platform # 系统[平台
process.arch # 系统架构
process.cwd() # 当前目录
process.chdir('/tmp') # 切换目录
```

process.nextTick()将在下一轮事件循环中调用

```js
process.nextTick(function () {
  console.log('nextTick callback!');
});
console.log('nextTick was set!');
```

```
nextTick was set!
nextTick callback!
```

程序即将退出时的回调函数

```js
process.on('exit', function (code) {
  console.log('about to exit with code: ' + code);
});
```

平台判断

```js
if (typeof(window) === 'undefined') {
  console.log('node.js');
} else {
  console.log('browser');
}
```








# 参考资料

[廖雪峰官方网站: node.js](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434502419592fd80bbb0613a42118ccab9435af408fd000) 

[粉丝日志: Nodejs学习路线图](http://blog.fens.me/nodejs-roadmap/) 

[粉丝日志: 从零开始nodejs系列文章](http://blog.fens.me/series-nodejs/) 

