---
title: Node.js学习笔记03 - http
categories:
  - Node.js
  - Node.js学习笔记
tags:
  - Node.js
---



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