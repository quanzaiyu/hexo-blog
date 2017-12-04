---
title: 基于node的Express应用程序框架
categories:
  - node
tags:
  - Express
---

# 前言

Express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。

使用 Express 可以快速地搭建一个完整功能的网站。

Express 框架核心特性：

- 可以设置中间件来响应 HTTP 请求。
- 定义了路由表用于执行不同的 HTTP 请求动作。
- 可以通过向模板传递参数来动态渲染 HTML 页面。



## 安装 Express

```bash
yarn add express
```

通常还会安装以下几个常用的模块:

```bash
yarn add body-parser cookie-parser multer
```

- **body-parser** - node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据。
- **cookie-parser** - 这就是一个解析Cookie的工具。通过req.cookies可以取到传过来的cookie，并把它们转成对象。
- **multer** - node.js 中间件，用于处理 enctype="multipart/form-data"（设置表单的MIME编码）的表单数据。

查看 express 使用的版本号

```bash
yarn list express
```



# Hello World

**app.js**

```js
let express = require('express')
let app = express()
 
app.use(express.static('public'))

app.get('/', (req, res) => {
   res.send('Hello World')
})

//  POST 请求
app.post('/', (req, res) => {
  console.log("主页 POST 请求")
  res.send('Hello POST')
})

//  /list_user 页面 GET 请求
app.get('/list_user', (req, res) => {
  console.log("/list_user GET 请求")
  res.send('用户列表页面')
})

// 对页面 abcd, abxcd, ab123cd, 等响应 GET 请求
// 字符 ?、+、* 和 () 是正则表达式的子集，- 和 . 在基于字符串的路径中按照字面值解释。
app.get('/ab*cd', (req, res) => {   
  console.log("/ab*cd GET 请求")
  res.send('正则匹配')
})

let server = app.listen(8081, () => {
  let host = server.address().address
  let port = server.address().port
  console.log("应用实例，访问地址为 http://%s:%s", host, port)
})

```

运行 `node app` ，访问 http://localhost:8081/ ，可以看到屏幕输出'Hello World'



## 路由

几种路由形式:

```js
// 对网站首页的访问返回 "Hello World!" 字样
app.get('/', function (req, res) {
  res.send('Hello World!');
});

// 网站首页接受 POST 请求
app.post('/', function (req, res) {
  res.send('Got a POST request');
});

// /user 节点接受 PUT 请求
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user');
});

// /user 节点接受 DELETE 请求
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user');
});
```

还有一种比较特殊的`app.all()`，不管使用 GET、POST、PUT、DELETE 或其他任何 [http 模块](https://nodejs.org/api/http.html#http_http_methods)支持的 HTTP 请求，句柄都会得到执行。

```js
app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...')
  next() // pass control to the next handler
})
```

### 查询字符串

> 查询字符串不是路由路径的一部分。

### 链式路由

```js
app.route('/book')
  .get(function(req, res) {
    res.send('Get a random book');
  })
  .post(function(req, res) {
    res.send('Add a book');
  })
  .put(function(req, res) {
    res.send('Update the book');
  });
```

### express.Router

创建模块化的路由文件

**book.js**

```js
var express = require('express');
var router = express.Router();

// 该路由使用的中间件
router.use(function timeLog(req, res, next) {
  console.log('Time: ', Date.now());
  next();
});
// 定义网站主页的路由
router.get('/', function(req, res) {
  res.send('Birds home page');
});
// 定义 about 页面的路由
router.get('/about', function(req, res) {
  res.send('About birds');
});

module.exports = router;
```

在入口文件 **app.js** 中加入

```js
var birds = require('./birds');
app.use('/birds', birds);
```

则可以发送 `/birds` 和 `/birds/about` 的请求。



## 请求和响应

路由中包含request 和 response 对象

```js
app.get('/', (req, res) => {
   res.send('Hello World')
})
```

### Request 对象

>  request 对象表示 HTTP 请求，包含了请求查询字符串，参数，内容，HTTP 头部等属性。

| 方法                    | 描述                                   |
| --------------------- | ------------------------------------ |
| req.path              | 获取请求路径                               |
| req.query             | 获取URL的查询参数串                          |
| req.route             | 获取当前匹配的路由                            |
| req.baseUrl           | 获取路由当前安装的URL路径                       |
| req.originalUrl       | 获取原始请求URL                            |
| req.params            | 获取路由的parameters                      |
| req.hostname          | 获取主机名                                |
| req.ip                | 获取IP地址                               |
| req.body              | 获得「请求主体」                             |
| req.cookies           | 获得Cookies                            |
| req.is()              | 判断请求头Content-Type的MIME类型             |
| req.get()             | 获取指定的HTTP请求头                         |
| req.accepts()         | 检查可接受的请求的文档类型                        |
| req.subdomains        | 获取子域名                                |
| req.app               | 当callback为外部文件时，用req.app访问express的实例 |
| req.protocol          | 获取协议类型                               |
| req.fresh / req.stale | 判断请求是否还「新鲜」                          |


### Response 对象

> response 对象表示 HTTP 响应，即在接收到请求时向客户端发送的 HTTP 响应数据。

| 方法                                       | 描述                             |
| ---------------------------------------- | ------------------------------ |
| [res.download()](http://www.expressjs.com.cn/4x/api.html#res.download) | 提示下载文件。                        |
| [res.end()](http://www.expressjs.com.cn/4x/api.html#res.end) | 终结响应处理流程。                      |
| [res.json()](http://www.expressjs.com.cn/4x/api.html#res.json) | 发送一个 JSON 格式的响应。               |
| [res.jsonp()](http://www.expressjs.com.cn/4x/api.html#res.jsonp) | 发送一个支持 JSONP 的 JSON 格式的响应。     |
| [res.redirect()](http://www.expressjs.com.cn/4x/api.html#res.redirect) | 重定向请求。                         |
| [res.render()](http://www.expressjs.com.cn/4x/api.html#res.render) | 渲染视图模板。                        |
| [res.send()](http://www.expressjs.com.cn/4x/api.html#res.send) | 发送各种类型的响应。                     |
| [res.sendFile](http://www.expressjs.com.cn/4x/api.html#res.sendFile) | 以八位字节流的形式发送文件。                 |
| [res.sendStatus()](http://www.expressjs.com.cn/4x/api.html#res.sendStatus) | 设置响应状态代码，并将其以字符串形式作为响应体的一部分发送。 |



## 404

**app.js** 中添加

```js
app.use(function(req, res, next) {
  res.status(404).send('Sorry cant find that!')
})
```

这样即可处理返回状态码为404的请求



## 错误处理器

**app.js** 中添加

```js
app.use(function(err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

这样就可以捕捉请求错误了



## 静态资源

```js
app.use(express.static('public'))
```

比如在站点根目录下存在一个名叫 `/public/logo.png`的文件

要访问之只需要输入 `http://localhost:8081/logo.png` 即可

### 挂载到虚拟路径

```js
app.use('/static', express.static('public'))
```

此时访问 `/public/logo.png`文件，则输入`http://localhost:8081/static/logo.png`

### 多个资源目录设置

```js
app.use(express.static('public'))
app.use(express.static('files'))
```

访问静态资源文件时，`express.static` 中间件会根据目录添加的顺序查找所需的文件。



## 请求的处理

**get.html**

```html
<html>

<body>
  <form action="http://127.0.0.1:8081/process_get" method="GET">
    First Name:
    <input type="text" name="first_name">
    <br> Last Name:
    <input type="text" name="last_name">
    <input type="submit" value="Submit">
  </form>
</body>

</html>
```

**post.html**

```html
<html>

<body>
  <form action="http://127.0.0.1:8081/process_post" method="POST">
    First Name:
    <input type="text" name="first_name">
    <br> Last Name:
    <input type="text" name="last_name">
    <input type="submit" value="Submit">
  </form>
</body>

</html>
```

**app.js** 中添加

```js
app.get('/get.html', function (req, res) {
  res.sendFile( __dirname + "/" + "get.html" );
})

app.get('/post.html', function (req, res) {
  res.sendFile( __dirname + "/" + "post.html" );
})

app.get('/process_get', function (req, res) {
  let response = {
    "first_name": req.query.first_name,
    "last_name": req.query.last_name
  }
  console.log(response)
  res.end(JSON.stringify(response))
})

// 创建 application/x-www-form-urlencoded 编码解析
let bodyParser = require('body-parser')
let urlencodedParser = bodyParser.urlencoded({ extended: false })
app.post('/process_post', urlencodedParser, function (req, res) {
  let response = {
    "first_name":req.body.first_name,
    "last_name":req.body.last_name
  }
  console.log(response);
  res.end(JSON.stringify(response))
})
```

访问 `http://localhost:8081/get.html` 和 `http://localhost:8081/post.html` 可以测试两种请求的结果。



## 文件上传

**uploader.html**

```html
<html>

<head>
  <title>文件上传表单</title>
</head>

<body>
  <h3>文件上传：</h3>
  选择一个文件上传:
  <br />
  <form action="/file_upload" method="post" enctype="multipart/form-data">
    <input type="file" name="image" size="50" />
    <br />
    <input type="submit" value="上传文件" />
  </form>
</body>

</html>
```

**app.js** 中添加

```js
var fs = require("fs")
var multer  = require('multer')
let bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({ extended: false }))
app.use(multer({ dest: '/tmp/'}).array('image'))

app.get('/uploader.html', function (req, res) {
  res.sendFile( __dirname + "/" + "uploader.html" );
})

app.post('/file_upload', function (req, res) {
  console.log(req.files[0]);  // 上传的文件信息
  let des_file = __dirname + "/" + req.files[0].originalname
  fs.readFile( req.files[0].path, function (err, data) {
    fs.writeFile(des_file, data, function (err) {
      if( err ){
        console.log( err )
      }else{
        response = {
          message:'File uploaded successfully', 
          filename:req.files[0].originalname
        }
      }
      console.log( response );
      res.end( JSON.stringify( response ) )
    })
  })
})
```

访问 `http://localhost:8081/uploader.html` 可以测试上传结果，在站点根目录下可以看到上传的文件。



## Cookie 管理

**app.js** 中添加

```js
let cookieParser = require('cookie-parser')
app.use(cookieParser())

app.get('/', (req, res) => {
   console.log("Cookies: ", req.cookies)
})
```

访问站点根，可以再控制台输出JSON形式的cookie





# Express 应用生成器

安装

```bash
yarn global add express-generator
```

帮助

```bash
express -h
```

构建项目

```bash
express <appName>
cd <appName>
yarn
yarn start
```

或者带环境变量启动

```bash
set DEBUG=myapp & npm start
```

浏览器输入 http://localhost:3000/ 即可开启站点








# 参考资料

[Runoob: Node.js Express 框架](http://www.runoob.com/nodejs/nodejs-express-framework.html) 

[Express基于 Node.js 平台，快速、开放、极简的 web 开发框架](http://www.expressjs.com.cn/) 



