---
title: 如何发布自己开发的npm包
categories:
  - Node.js
tags:
  - npm
---



前一段时间开发了一些基于Vue的插件，感觉还挺好用的，就发布到npm，希望大家都能使用。

下面说一下发布npm 的流程:



# 注册一个npm账号

进入网址: https://www.npmjs.com 注册一个npm账号。

本人的npm地址为: [https://www.npmjs.com/~quanzaiyu]( https://www.npmjs.com/~quanzaiyu )  

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/npm/001.png)

注册后如果需要进行头像设置，需要到 https://en.gravatar.com 注册一个账号(使用的是WordPress账号)，添加头像。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/npm/002.png)



# 开发一个npm包

## 初始化仓库

每一个npm包都需要一个`package.json`文件，进行常规配置。

首先进入需要发布npm的目录，输入

```bash
npm init
```

进行包初始化，自动生成一个`package.json`文件，填写一些简单的选项，包括: 包名、版本号、主入口文件、描述、作者、脚本 等。

最终文件大致如下:

```json
{
  "name": "qzy-npm-test",
  "version": "1.0.0",
  "description": "一个npm测试包",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "quanzaiyu",
  "license": "ISC"
}
```



## 主入口文件

可以看到，在`package.json`中指定主入口文件为`index.js`，那么，就得在项目下创建一个名为`index.js`的文件。比如:

```js
function hello(name){
  console.log("hello "+ name);
}
exports.hello = hello;
```

可以看到，此文件导出了一个名叫`hello`的函数。



## 测试此包

将整个文件夹丢到`node_modules`目录下，在`node_modules`同级目录下使用`npm init` 创建 `package.json`，内容大体如下:

```json
{
  "name": "projects",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "dependencies": {
    "qzy-npm-test": "^1.0.1"
  },
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

再在同级目录下创建一个`test.js`，内容如下:

```js
var h = require('qzy-npm-test');
h.hello('Jarrick');
```

执行`node test`，控制台输出`hello Jarrick`。说明此包测试成功。



创建的目录结构如下:

```
projects/ # 测试项目目录
	node_modules/ # 包目录
		qzy-npm-test/ # 插件目录
			index.js # 插件入口文件
			package.json # 插件配置文件
	test.js # 测试文件
	package.json # 测试配置文件
```



# 发布一个npm包

以下操作都在 `projects/node_modules/qzy-npm-test/` 目录下进行。

## 添加npm用户

使用之前注册的npm账号进行登录

```bash
$ npm adduser
username: xxx
password: xxx
email: xxx
```

## 发布npm包

```bash
$ npm publish
```

发布后可在自己的npm主页看到

## 更新npm包

如果之后修改过此包，需要修改`package.json`中的版本号字段`version`，使其大于当前版本，然后`npm publish`即可。

如果未更改版本号，会报错:

```
npm ERR! publish Failed PUT 403
npm ERR! code E403
npm ERR! You cannot publish over the previously published version 1.0.0. : qzy-npm-test

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\quanzaiyu\AppData\Roaming\npm-cache\_logs\2017-09-12T07_59_18_829Z-debug.log
```

修改版本号后则成功:

```bash
$ npm publish
+ qzy-npm-test@1.0.1
```



## 删除npm包

```bash
$ npm unpublish
$ npm unpublish --force
```



# 安装使用npm包

安装使用方法很简单，跟以前安装npm包同样的使用即可。

```bash
npm i <packageName>
```

比如安装刚才发布的包:

```bash
npm i qzy-npm-test --save
```

使用刚才发布的包:

```js
let a = require('qzy-npm-test')
a.hello('qzy')
```

执行`node index`即可看见输出了`hello qzy`

详细的使用请访问本人的npm: [https://www.npmjs.com/~quanzaiyu]( https://www.npmjs.com/~quanzaiyu )  



# 参考资料

[npm-参考手册](https://segmentfault.com/a/1190000009315989) 

[手把手教你用npm发布一个包](http://www.jianshu.com/p/36d3e0e00157) 

[怎样删除npm里已经发布的包？](https://segmentfault.com/q/1010000009389901) 