---
title: 我的开发之路系列 - Node.js学习指南
categories:
  - Node.js
tags:
  - Node.js
---

> 本文已制作成完整电子书发布，这里只是简单介绍python3的常用操作，更多详情请查看《<u>[我的开发之路系列 - node.js学习指南](http://book.node.xiaoyulive.top)</u>》

<div style='text-align: center;'>
<a href='http://book.node.xiaoyulive.top'>
  <img src='http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/node.js/node.jpg?x-oss-process=style/thumb'/>
</a>
</div>



# 前言

本书为本人在学习node.js过程中，从大量相关书籍和资料中汇总出的node.js开发精髓，其中包含大量实例，方便查阅，特在此分享，希望对大家有用。

其实在写本书的时候，我很纠结，到底要不要涉及js的基础知识。说实话，写基础知识是一件很无聊的事，而且理论上达到学习node阶段的同学应该是js还不错的同学，不过经过一番取舍，还是决定写一些ECMAScript的常用标准语法（注意JavaScript与ECMAScript的关系，ECMAScript是JavaScript的标准规范，JavaScript是ECMAScript的具体实现），便于查阅，顺便也复习一下js的知识。

我本人也是一个 node.js 的学习者， 不敢说自己有多高的水平， 只是在学习过程中做了详细的笔记而已。 在写作过程中难免有不尽人意的地方， 希望大家在阅读中提出勘误， 本人会极力纠正，谢谢。



# node.js简介

Node.js 就是运行在服务端的 JavaScript，它是一个基于 Chrome V8 引擎的 JavaScript 运行时。 Node.js 使用高效、轻量级的事件驱动、非阻塞 I/O 模型。它的包生态系统，npm，是目前世界上最大的开源库生态系统。



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



# 读写文件

## 异步读文件

```js
'use strict';

var fs = require('fs');

fs.readFile('file1.txt', 'utf-8', function (err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
    }
});

console.log('after')
```

`fs.readFile`将读取一个文件，第二参数设置文件编码，第三参数为回调函数，接收一个`err`和`data`，当正常读取时，`err`参数为`null`，`data`参数为读取到的String。当读取发生错误时，`err`参数代表一个错误对象，`data`为`undefined`。

由于是异步IO，将先打印`after`，等待读取文件完毕，再打印出文件内容。



### 二进制文件的读取

```js
'use strict';

var fs = require('fs');

fs.readFile('100.jpg', function (err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
        console.log(data.length + ' bytes');
    }
});
```

当读取二进制文件时，不传入文件编码时，回调函数的`data`参数将返回一个`Buffer`对象。在Node.js中，`Buffer`对象就是一个包含零个或任意个字节的数组（注意和Array不同）。

`Buffer`对象可以和String作转换，例如，把一个`Buffer`对象转换成String：

```js
// Buffer -> String
var text = data.toString('utf-8');
console.log(text);
```

或者把一个String转换成`Buffer`：

```js
// String -> Buffer
var buf = Buffer.from(text, 'utf-8');
console.log(buf);
```



## 同步读文件

除了标准的异步读取模式外，`fs`也提供相应的同步读取函数。同步读取的函数和异步函数相比，多了一个`Sync`后缀，并且不接收回调函数，函数直接返回结果。

用`fs`模块同步读取一个文本文件的代码如下：

```js
'use strict';

var fs = require('fs');

try {
  var data = fs.readFileSync('100.jpg', 'utf-8');
  console.log(Buffer.from(data, 'utf-8'));
} catch (err) {
  console.log(err)
}
console.log('after')
```

原异步调用的回调函数的`data`被函数直接返回，函数名需要改为`readFileSync`，其它参数不变。

如果同步读取文件发生错误，则需要用`try...catch`捕获该错误。



## 写文件

将数据写入文件是通过`fs.writeFile()`实现的：

```js
'use strict';

var fs = require('fs');

var data = 'Hello, Node.js';
fs.writeFile('output.txt', data, function (err) {
    if (err) {
        console.log(err);
    } else {
        console.log('ok.');
    }
});
```

`writeFile()`的参数依次为文件名、数据和回调函数。如果传入的数据是String，默认按UTF-8编码写入文本文件，如果传入的参数是`Buffer`，则写入的是二进制文件。回调函数由于只关心成功与否，因此只需要一个`err`参数。

和`readFile()`类似，`writeFile()`也有一个同步方法，叫`writeFileSync()`：

```js
'use strict';

var fs = require('fs');

var data = 'Hello, Node.js';
fs.writeFileSync('output.txt', data);
```



# HTTP服务器

`request`对象封装了HTTP请求，我们调用`request`对象的属性和方法就可以拿到所有HTTP请求的信息；

`response`对象封装了HTTP响应，我们操作`response`对象的方法，就可以把HTTP响应返回给浏览器。

用Node.js实现一个HTTP服务器程序:

```js
'use strict';

// 导入http模块:
var http = require('http');

// 创建http server，并传入回调函数:
var server = http.createServer(function (request, response) {
    // 回调函数接收request和response对象,
    // 获得HTTP请求的method和url:
    console.log(request.method + ': ' + request.url);
    // 将HTTP响应200写入response, 同时设置Content-Type: text/html:
    response.writeHead(200, {'Content-Type': 'text/html'});
    // 将HTTP响应的HTML内容写入response:
    response.end('<h1>Hello world!</h1>');
});

// 让服务器监听2000端口:
server.listen(2000);

console.log('Server is running at http://127.0.0.1:2000/');
```

在命令行运行该程序并打开浏览器访问`http://127.0.0.1:2000/`，将在浏览器输出`Hello world!`



# 文件服务器

## url解析

```js
'use strict';

var url = require('url');

console.log(url.parse('http://user:pass@host.com:8080/path/to/file?query=string#hash'));
```

输出

```js
Url {
  protocol: 'http:',
  slashes: true,
  auth: 'user:pass',
  host: 'host.com:8080',
  port: '8080',
  hostname: 'host.com',
  hash: '#hash',
  search: '?query=string',
  query: 'query=string',
  pathname: '/path/to/file',
  path: '/path/to/file?query=string',
  href: 'http://user:pass@host.com:8080/path/to/file?query=string#hash' }
```

## path解析

使用`path`模块可以正确处理操作系统相关的文件路径。在Windows系统下，返回的路径类似于`C:\Users\michael\static\index.html`。

```js
'use strict';

var path = require('path');

// 解析当前目录:
var workDir = path.resolve('.');

// 组合完整的文件路径:当前目录+'pub'+'index.html':
var filePath = path.join(workDir, 'pub', 'index.html');
```



## 实现文件服务器

**file_server.js**

```js
'use strict';

var
    fs = require('fs'),
    url = require('url'),
    path = require('path'),
    http = require('http');

// 从命令行参数获取root目录，默认是当前目录:
var root = path.resolve(process.argv[2] || '.');

console.log('Static root dir: ' + root);

// 创建服务器:
var server = http.createServer(function (request, response) {
    // 获得URL的path
    var pathname = url.parse(request.url).pathname;
    console.log('pathname: ', pathname)
    // 获得对应的本地文件路径
    var filepath = path.join(root, pathname);
    console.log('filepath: ', filepath)
    // 获取文件状态:
    fs.stat(filepath, function (err, stats) {
        if (!err && stats.isFile()) {
            // 没有出错并且文件存在:
            console.log('200 ' + request.url);
            // 发送200响应:
            response.writeHead(200);
            // 将文件流导向response:
            fs.createReadStream(filepath).pipe(response);
        } else {
            // 出错了或者文件不存在:
            console.log('404 ' + request.url);
            // 发送404响应:
            response.writeHead(404);
            response.end('404 Not Found');
        }
    });
});

server.listen(2000);

console.log('Server is running at http://127.0.0.1:2000/');
```

没有必要手动读取文件内容。由于`response`对象本身是一个`Writable Stream`，直接用`pipe()`方法就实现了自动读取文件内容并输出到HTTP响应。

在命令行直接运行 `nodefile_server `，将当前工作目录作为文件服务器，如果当前目录下有一个叫`index.html`的文件，在浏览器中输入`http://localhost:2000/index.html`即可访问。

在命令行运行`node file_server.js /path/to/dir`，把`/path/to/dir`改成你本地的一个有效的目录，然后在浏览器中输入`http://localhost:2000/index.html`。

在浏览器输入`http://localhost:8080/`时，会返回404，原因是程序识别出HTTP请求的不是文件，而是目录。




# 参考资料

[廖雪峰官方网站: node.js](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434502419592fd80bbb0613a42118ccab9435af408fd000) 

[粉丝日志: Nodejs学习路线图](http://blog.fens.me/nodejs-roadmap/) 

[粉丝日志: 从零开始nodejs系列文章](http://blog.fens.me/series-nodejs/) 

