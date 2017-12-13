---
title: Node.js应用篇 - 新一代Node服务器框架KOA
categories:
  - Node.js
  - Node.js应用篇
tags:
  - KOA
---



# 简介

**为什么说是新一代?**

新一代是koa，那上一代呢? 上一代就是大名鼎鼎的Express，koa经过了两个大版本，比起koa1，koa2使用了ES7进行开发，完全使用Promise并配合`async`来实现异步。

本文编写的时候使用的是koa2



# 开始使用

## 简单使用

新建一个项目名叫`myapp`，进入此目录

```bash
npm init
npm i koa -s
touch index.js
vim idnex.js
```

编写:

```js
var koa = require('koa');
var app = new koa();
app.use(function* () { 
  this.body = 'Hello World';
});
app.listen(3000);
```

运行`node index`，在浏览器输入`http://localhost:3000/`即可看到输出Hello World。



## 使用脚手架

### 安装

```bash
yarn global add koa-generator
koa2 <my-app>
cd <my-app> && npm i
npm start
```



### 项目解析

#### 路由管理

**app.js**

```js
// 引入路由文件
const index = require('./routes/index')
const users = require('./routes/users')

// 加载路由
app.use(index.routes(), index.allowedMethods())
app.use(users.routes(), users.allowedMethods())
```

**routes/index**

```js
const router = require('koa-router')()

// 访问 http://localhost:3000/ 即可输出 Hello Koa 2!，渲染 pug 模板
router.get('/', async (ctx, next) => {
  await ctx.render('index', {
    title: 'Hello Koa 2!'
  })
})

// 访问 http://localhost:3000/string 直接输出字符串
router.get('/string', async (ctx, next) => {
  ctx.body = 'koa2 string'
})

// 访问 http://localhost:3000/string 直接输出json
router.get('/json', async (ctx, next) => {
  ctx.body = {
    title: 'koa2 json'
  }
})

module.exports = router
```

**routes/users**

```js
const router = require('koa-router')()

// 添加路由前缀
router.prefix('/users')

// 访问 http://localhost:3000/users 直接输出字符串
router.get('/', function (ctx, next) {
  ctx.body = 'this is a users response!'
})

// 访问 http://localhost:3000/users/bar 直接输出字符串
router.get('/bar', function (ctx, next) {
  ctx.body = 'this is a users/bar response'
})

module.exports = router
```



#### 页面渲染

**app.js**

```js
router.get('/', async (ctx, next) => {
  await ctx.render('index', {
    title: 'Hello Koa 2!'
  })
})
```

- await是ES6的关键字，用于把异步代码同步化，就不再写回调函数了(callback)。
- ctx.render()函数，用于加载渲染引擎。

**views/index.pug**

```jade
extends layout

block content
  h1= title
  p Welcome to #{title}
```



#### 日志分析

这是一个中间件，每一次的浏览行为，都会产生一条服务器日志，用于记录用户的访问情况。我们后台通过命令行启动后，后台的服务器就会一直存活着，接收浏览器的请求，同时产生日志。

```js
app.use(async (ctx, next) => {
  const start = new Date()
  await next()
  const ms = new Date() - start
  console.log(`${ctx.method} ${ctx.url} - ${ms}ms`)
})
```

最后要说的就是服务器日志了，每一次的浏览行为，都会产生一条服务器日志，用于记录用户的访问情况。我们后台通过命令行启动后，后台的服务器就会一直存活着，接收浏览器的请求，同时产生日志。

日志中，200表示正常访问，404是没有对应的URL错误。可以看到每次访问的路径都被记录了，包括后台的路径和css的文件路径，还包括了访问协议，响应时间，页面大小等。

我们可以自定义日志格式，记录更多的信息，也可以记录对自己有用的信息。这样我们就构建出一个最小化的web应用了。



#### 中间件

koa把很多async函数组成一个处理链，每个async函数都可以做一些自己的事情，然后用`await next()`来调用下一个async函数。我们把每个async函数称为middleware，这些middleware可以组合起来，完成很多有用的功能。

```js
app.use(async (ctx, next) => {
    if (await checkUserPermission(ctx)) {
        await next();
    } else {
        ctx.response.status = 403;
    }
});

app.use(async (ctx, next) => {
    console.log(`${ctx.request.method} ${ctx.request.url}`); // 打印URL
    await next(); // 调用下一个middleware
});

app.use(async (ctx, next) => {
    const start = new Date().getTime(); // 当前时间
    await next(); // 调用下一个middleware
    const ms = new Date().getTime() - start; // 耗费时间
    console.log(`Time: ${ms}ms`); // 打印耗费时间
});
```

middleware的顺序很重要，也就是调用`app.use()`的顺序决定了middleware的顺序。

此外，如果一个middleware没有调用`await next()`，会怎么办？答案是后续的middleware将不再执行了。

比如上述代码中，检测用户权限，如果权限不足，则直接返回403，不再执行后续操作。

`ctx`对象有一些简写的方法，例如`ctx.url`相当于`ctx.request.url`，`ctx.type`相当于`ctx.response.type`



# 中间件开发

## 静态资源处理

**static-files.js**

```js
const path = require('path');
const mime = require('mime');
const fs = require('mz/fs');

// url: 类似 '/static/'
// dir: 类似 __dirname + '/static'
function staticFiles(url, dir) {
    return async (ctx, next) => {
        let rpath = ctx.request.path;
        // 判断是否以指定的url开头:
        if (rpath.startsWith(url)) {
            // 获取文件完整路径:
            let fp = path.join(dir, rpath.substring(url.length));
            // 判断文件是否存在:
            if (await fs.exists(fp)) {
                // 查找文件的mime:
                ctx.response.type = mime.getType(rpath);
                // 读取文件内容并赋值给response.body:
                ctx.response.body = await fs.readFile(fp);
            } else {
                // 文件不存在:
                ctx.response.status = 404;
            }
        } else {
            // 不是指定前缀的URL，继续处理下一个middleware:
            await next();
        }
    };
}

module.exports = staticFiles;
```

注意到使用到了`mime`，模块，用于解析mime类型，之前一直是使用`mime.lookup`，使用的时候疯狂报错，查看了官方文档，才知道换成了`mime.getType`，官方文档永远是最好的参考。 [参考链接](https://www.npmjs.com/package/mime) 

**app.js**

```js
const staticFiles = require('./static-files');
app.use(staticFiles('/static/', __dirname + '/static'));
```

在浏览器输入`http://localhost:3000/static/xxx`即可直接访问静态资源。




# 参考资料

[廖雪峰: koa入门](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001471087582981d6c0ea265bf241b59a04fa6f61d767f6000) 

[粉丝日志: 新一代Node.js的Web开发框架Koa2](http://blog.fens.me/nodejs-koa2/) 

[mime模块响应或设置Node.js的Content-Type头](http://www.cnblogs.com/jeacy/p/6992435.html) 