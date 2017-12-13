---
title: Node.js应用篇 - 基于node的Express应用程序框架
categories:
  - Node.js
  - Node.js应用篇
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


本文写作时使用的Express版本为4.x


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



# 中间件

Express 是一个自身功能极简，完全是由路由和中间件构成一个的 web 开发框架：从本质上来说，一个 Express 应用就是在调用各种中间件。

*中间件（Middleware）* 是一个函数，它可以访问请求对象（[request object](http://www.expressjs.com.cn/4x/api.html#req) (`req`)）, 响应对象（[response object](http://www.expressjs.com.cn/4x/api.html#res) (`res`)）, 和 web 应用中处于请求-响应循环流程中的中间件，一般被命名为 `next` 的变量。

中间件的功能包括：

- 执行任何代码。
- 修改请求和响应对象。
- 终结请求-响应循环。
- 调用堆栈中的下一个中间件。

如果当前中间件没有终结请求-响应循环，则必须调用 `next()` 方法将控制权交给下一个中间件，否则请求就会挂起。

Express 应用可使用如下几种中间件：

- [应用级中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.application)
- [路由级中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.router)
- [错误处理中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.error-handling)
- [内置中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.built-in)
- [第三方中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.third-party)

使用可选则挂载路径，可在应用级别或路由级别装载中间件。另外，你还可以同时装在一系列中间件函数，从而在一个挂载点上创建一个子中间件栈。

## 应用级中间件

应用级中间件绑定到 [app 对象](http://www.expressjs.com.cn/4x/api.html#app) 使用 `app.use()` 和 `app.METHOD()`， 其中， `METHOD` 是需要处理的 HTTP 请求的方法，例如 GET, PUT, POST 等等，全部小写。

```js
var app = express();

// 没有挂载路径的中间件，应用的每个请求都会执行该中间件
app.use(function (req, res, next) {
  console.log('Time:', Date.now());
  next();
});

// 一个中间件栈，对任何指向 /user/:id 的 HTTP 请求打印出相关信息
app.use('/user/:id', function(req, res, next) {
  console.log('Request URL:', req.originalUrl);
  next();
}, function (req, res, next) {
  console.log('Request Type:', req.method);
  next();
}, function (req, res, next) {
  console.log('Request Test');
  next();
});

// 路由和句柄函数(中间件系统)，处理指向 /user/:id 的 GET 请求
app.get('/user/:id', function (req, res, next) {
  res.send('USER');
});
```

注意，必须使用`next()`才能使应用程序继续调用下一个中间件，如果不写，则请求循环将终止，之后的中间件将不再执行。

## 跳过中间件

如果需要在中间件栈中跳过剩余中间件，调用 `next('route')` 方法将控制权交给下一个路由。 **注意**： `next('route')` 只对使用 `app.VERB()` 或 `router.VERB()` 加载的中间件有效。

```js
// 一个中间件栈，处理指向 /user/:id 的 GET 请求
app.get('/user/:id', function (req, res, next) {
  // 如果 user id 为 0, 跳到下一个路由
  if (req.params.id == 0) next('route');
  // 否则将控制权交给栈中下一个中间件
  else next(); //
}, function (req, res, next) {
  // 渲染常规页面
  res.render('regular');
});

// 处理 /user/:id， 渲染一个特殊页面
app.get('/user/:id', function (req, res, next) {
  res.render('special');
});
```

## 路由级中间件express.Router

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

// 一个中间件栈，显示任何指向 /user/:id 的 HTTP 请求的信息
router.use('/user/:id', function(req, res, next) {
  console.log('Request URL:', req.originalUrl);
  next();
}, function (req, res, next) {
  console.log('Request Type:', req.method);
  next();
});

// 一个中间件栈，处理指向 /user/:id 的 GET 请求
router.get('/user/:id', function (req, res, next) {
  // 如果 user id 为 0, 跳到下一个路由
  if (req.params.id == 0) next('route');
  // 负责将控制权交给栈中下一个中间件
  else next(); //
}, function (req, res, next) {
  // 渲染常规页面
  res.render('regular');
});

// 处理 /user/:id， 渲染一个特殊页面
router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id);
  res.render('special');
});

module.exports = router;
```

在入口文件 **app.js** 中加入

```js
var app = express();
var birds = require('./birds');
app.use('/person', birds);
```

则可以发送 `/person和 ` `/person/about` 的请求，也可处理 `/person/user/:id`的请求，并执行中间件操作。



## 404的处理

**app.js** 中添加

```js
app.use(function(req, res, next) {
  res.status(404).send('Sorry cant find that!')
})
```

这样即可处理返回状态码为404的请求



## 错误处理中间件

**app.js** 中添加

```js
function errorHandler(err, req, res, next) {
  if (res.headersSent) {
    return next(err);
  }
  res.status(500);
  res.render('error', { error: err });
}
```

模板为`/views/error.jade`，这样就可以捕捉请求错误了



## 第三方中间件

参考 [第三方中间件](http://www.expressjs.com.cn/resources/middleware.html) 获取 Express 中经常用到的第三方中间件列表。

常用的第三方中间件有:

**cookie-parser**、**body-parser**、**multer**



# 静态资源

`express.static` 是 Express 唯一内置的中间件。它基于 [serve-static](https://github.com/expressjs/serve-static)，负责在 Express 应用中提托管静态资源。

```js
app.use(express.static('public'))
```

比如在站点根目录下存在一个名叫 `/public/logo.png`的文件

要访问之只需要输入 `http://localhost:8081/logo.png` 即可

## 挂载到虚拟路径

```js
app.use('/static', express.static('public'))
```

此时访问 `/public/logo.png`文件，则输入`http://localhost:8081/static/logo.png`

## 多个资源目录设置

```js
app.use(express.static('public'))
app.use(express.static('files'))
```

访问静态资源文件时，`express.static` 中间件会根据目录添加的顺序查找所需的文件。



# 请求的处理

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



# 文件上传

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



# Cookie 管理

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



# 模板引擎

express默认使用jade(pug) 模板引擎。

设置模板引擎:

```js
app.set('view engine', 'jade')
```

在`views`目录下新建`index.jade`:

```jade
html
  head
    title!= title
  body
    h1!= message
```

渲染模板:

```js
app.get('/', function (req, res) {
  res.render('index', { title: 'Hey', message: 'Hello there!'})
})
```



# 调试

打印所有调试信息，当应用收到请求时，能看到 Express 代码中打印出的日志。

```
set DEBUG=express:* & npm start
```

设置 DEBUG 的值为 `express:router`，只查看路由部分的日志；

设置 DEBUG 的值为 `express:application`，只查看应用部分的日志。

可通过逗号隔开的名字列表来指定多个调试命名空间:

```
DEBUG=http,mail,express:* npm start
```



# 代理

当在代理服务器之后运行 Express 时，请将应用变量 `trust proxy` 设置（使用 [app.set()](http://www.expressjs.com.cn/4x/api.html#app.set)）为下述列表中的一项。

- Boolean: 

  如果为 `true`，客户端 IP 地址为 `X-Forwarded-*` 头最左边的项。

  如果为 `false`, 应用直接面向互联网，客户端 IP 地址从 `req.connection.remoteAddress` 得来，这是默认的设置。

- IP 地址

  如: 

  ```js
  app.set('trust proxy', 'loopback') // 指定唯一子网
  app.set('trust proxy', 'loopback, 123.123.123.123') // 指定子网和 IP 地址
  app.set('trust proxy', 'loopback, linklocal, uniquelocal') // 指定多个子网
  app.set('trust proxy', ['loopback', 'linklocal', 'uniquelocal']) // 使用数组指定多个子网
  ```

- Number: 将代理服务器前第 `n` 跳当作客户端。

- Function

  如:

  ```js
  app.set('trust proxy', function (ip) {
    if (ip === '127.0.0.1' || ip === '123.123.123.123') return true; // 受信的 IP 地址
    else return false;
  })
  ```

详见: [为 Express 设置代理](http://www.expressjs.com.cn/guide/behind-proxies.html) 



# 数据库

为 Express 应用添加连接数据库的能力，只需要加载相应数据库的 Node.js 驱动即可。

## MySQL

```bash
npm install mysql
```

数据库链接语句如下:

```js
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : '',
  password : ''
});

connection.connect();

connection.query('SELECT * from user_table', function(err, rows, fields) {
  if (err) throw err;
  console.log('The row is: ', rows[0].name);
});

connection.end();
```

更多数据库驱动详见: [集成数据库](http://www.expressjs.com.cn/guide/database-integration.html#mysql) 




# 参考资料

[Runoob: Node.js Express 框架](http://www.runoob.com/nodejs/nodejs-express-framework.html) 

[Express基于 Node.js 平台，快速、开放、极简的 web 开发框架](http://www.expressjs.com.cn/) 



