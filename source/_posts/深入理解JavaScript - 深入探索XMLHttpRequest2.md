---
title: 深入理解JavaScript - 深入探索XMLHttpRequest2
categories:
  - 前端开发
  - 深入理解JavaScript
tags:
  - XMLHttpRequest
---



# 前言

web2.0出现很多年了，`XMLHttpRequest`也由此诞生，一代的`XMLHttpRequest`使用至今出现了诸多不足。作为其升级版的二代 `XMLHttpRequest` 引入了大量的新功能（例如跨源请求、上传进度事件以及对上传/下载二进制数据的支持等），而且 `AJAX` 可以与很多尖端的 `HTML5 API` 一起使用。



# 传统使用

## 请求一段文本内容:

```js
var xhr;
var url = 'http://localhost/test.txt';
if (window.XMLHttpRequest) {
  //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
  xhr = new XMLHttpRequest();
} else {
  // IE6, IE5 浏览器执行代码
  xhr = new ActiveXObject("Microsoft.XMLHTTP");
}
xhr.open('GET', url, true);
xhr.responseType = 'text';
xhr.send();
xhr.onreadystatechange = function () {
  console.log(xhr)
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      //处理数据
      console.log(xhr.responseText)
    } else {
      //其它操作
      console.log('error')
    }
  }
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0038.png)

正常的请求流程为:

1. 判断浏览器是否包含`XMLHttpRequest`对象，再创建一个浏览器对应的`XMLHttpRequest`(IE为`ActiveXObject`)对象
2. 使用`open`方法打开一个链接，指定请求方式和是否异步请求
3. 指定`responseType`返回数据类型为`text`，不指定则默认为`empty string` 
4. 使用`send`发送请求
5. 监听`onreadystatechange`事件，判断其`readyState`是否为4且`status`是否为200，如果是则代表请求成功
6. 使用`responseText`返回请求的数据



## 请求JSON格式的数据

```js
var url = 'http://localhost/test.json';
var xhr = new XMLHttpRequest();
xhr.open('GET', url, true);
xhr.responseType = 'json';
xhr.send();
xhr.onreadystatechange = function () {
  console.log(xhr)
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      console.log(xhr.response)
    }
  }
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0045.png)

请求`JSON`数据，返回的`response`就为此`JSON`的内容。



## 针对数据乱码的处理

在请求头中添加设置编码的方法，如

```js
xhr.setRequestHeader("Content-Type","charset=gb2312")
```



# 请求 - send

使用`XMLHttpRequest`发送数据，需要使用`POST`的请求方式。

## DOMString

发送一段普通的字符串。

```js
var url = 'http://localhost/getInfo.php';

var xhr = new XMLHttpRequest();
xhr.open('POST', url, true);
xhr.responseType = 'json';
xhr.send('Hello Ajax');
xhr.onload = function(e) {
  console.log(e)
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0051.png)



## JSON

发送一段json数据，注意发送之前需要使用`JSON.stringify()`将`json`字符串化。

```json
var url = 'http://localhost/getInfo.php';

var json = {
  name: 'xiaoyu',
  age: 18
}

var xhr = new XMLHttpRequest();
xhr.open('POST', url, true);
xhr.responseType = 'json';
xhr.send(JSON.stringify(json));
xhr.onload = function(e) {
  console.log(e)
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0047.png)



## FormData

XMLHttpRequest2添加了一个新的接口`FormData`。利用`FormData`对象，我们可以通过JavaScript用一些键值对来模拟一系列表单控件，我们还可以使用XMLHttpRequest的`send()`方法来异步的提交这个”表单”。比起普通的ajax, 使用`FormData`的最大优点就是我们可以异步上传一个二进制文件。

```js
var url = 'http://localhost/getInfo.php';
var formData = new FormData();

formData.append("name", 'xiaoyu');
formData.append("age", 22); // 注意，传递的时候会作为字符串处理

var xhr = new XMLHttpRequest();
xhr.open('POST', url, true);
xhr.responseType = 'json';
xhr.send(formData);
xhr.onload = function(e) {
  console.log(e)
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0048.png)



## Blob和ArrayBuffer

暂未研究。



# 响应 - 事件

## 处理方式

### 通过onreadystatechange事件处理

```js
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      //处理数据
    } else {
      //其它操作
    }
  }
}
```



### 通过onload 事件处理

```js
xhr.onload = function() {
  if (xhr.status === 200) {
    //处理数据
  } else {
    //其它操作
  }
}
```



### event对象

同普通的交互事件一样，`xhr`的事件中也包含一个`event`对象:

```js
xhr.onload = function(e) {
  console.log(e)
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0046.png)

可以看到此对象中也包含响应的数据，不过一般都不会使用`event`去获取数据。



## readyState、status和statusText

### readyState

当一个XMLHttpRequest初次创建时，这个属性的值是从0开始，知道接收完整的HTTP响应，这个值增加到4。有五种状态：

- 状态**0** (未初始化)： (XMLHttpRequest)对象已经创建或已被abort()方法重置，但还没有调用open()方法；
- 状态**1** (载入)：已经调用open() 方法，但是send()方法未调用，尚未发送请求；
- 状态**2** (载入完成)： send()方法已调用，HTTP请求已发送到web服务器，请求已经发送完成，未接收到响应；
- 状态**3** (交互)：所有响应头部都已经接收到。响应体开始接收但未完成，即可以接收到部分响应数据；
- 状态**4** (完成)：已经接收到了全部数据，并且连接已经关闭。

`onreadystatechange`事件可以监测到`readyState`的状态变化。



### status

这是HTTP的状态码，200为成功，404为找不到页面，500为服务器错误。



### statusText

`statusText`表示HTTP响应状态的描述文本，即OK、Not Found等



## getAllResponseHeaders和getResponseHeader

`xhr.getAllResponseHeaders()`可以获取响应头的所有内容

`xhr.getResponseHeader(name)`可以获取指定的响应头内容

如:

```js
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      console.log(xhr.getAllResponseHeaders())
      console.log(xhr.getResponseHeader('date'))
      console.log(xhr.response)
    } else {
      console.log('error')
    }
  }
}
```



# 响应 - responseType

| 类型           | 描述         |
| ------------ | ---------- |
| empty string | 空字符串，这是默认值 |
| arraybuffer  | 二进制缓冲数组    |
| blob         | 二进制大对象     |
| document     | 文档类型       |
| json         | JSON类型     |
| text         | 文本类型       |

注意:

当`responseType`为`text`或者`empty string`类型时可以使用`responseText`属性，为其它类型时调用`responseText`会发生异常；

当`responseType`为`document`或者`empty string`类型时可以使用`responseXML`属性，为其它类型时调用`responseXML`会发生异常；

当`responseType`不是`empty string`、`text`、`document`类型时，需要转换成具体的类型进行解析。



## responseType新类型 - Blob

前段时间做项目，需要请求二进制数据，在服务器中存储的是多媒体文件形式。此时需要将`responseType`指定为`blob`，才能正确接收。如:

```js
var url = 'http://localhost/test.amr';
var xhr = new XMLHttpRequest();
xhr.open('GET', url, true);
xhr.responseType = 'blob';
xhr.send();
xhr.onreadystatechange = function () {
  console.log(xhr)
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      console.log(xhr.response)
    }
  }
}
```

此时可以看到控制台打印出:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0037.png)

注意到此时`xhr`没有了`responseText`属性，而是直接使用`response`接收，返回的是一个`Blob`二进制数据。



### 处理图像类型文件

```js
var url = 'http://localhost/test.png';
window.URL = window.URL || window.webkitURL;

var xhr = new XMLHttpRequest();
xhr.open('GET', url, true);
xhr.responseType = 'blob';
xhr.send();
xhr.onload = function() {
  var blob = xhr.response;
  var img = document.createElement('img');
  img.onload = function(e) {
    window.URL.revokeObjectURL(img.src);
  }
  img.src = window.URL.createObjectURL(blob);
  document.body.appendChild(img);
}
```

**[URL.createObjectURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL) ** 会创建一个 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，其中包含一个表示参数中给出的对象的URL。这个 URL 的生命周期和创建它的窗口中的 [`document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 绑定。这个新的URL 对象表示指定的 [`File`](https://developer.mozilla.org/zh-CN/docs/Web/API/File) 对象或 [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象。

**[URL.revokeObjectURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/revokeObjectURL) **静态方法用来释放一个之前通过调用 [`URL.createObjectURL()`](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL) 创建的已经存在的 URL 对象。当你结束使用某个 URL 对象时，应该通过调用这个方法来让浏览器知道不再需要保持这个文件的引用了。

你可以在sourceopen被处理之后的任何时候调用revokeObjectURL()。这是因为createObjectURL()仅仅意味着将一个媒体元素的src属性关联到一个 [`MediaSource`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaSource) 对象。调用`revokeObjectURL()` 使这个潜在的对象保留在原来的地方，允许平台在合适的时机进行垃圾收集。



### 处理音频类型文件

```js
var url = 'http://localhost/audio.mp3';
window.URL = window.URL || window.webkitURL;

var xhr = new XMLHttpRequest();
xhr.open('GET', url, true);
xhr.responseType = 'blob';
xhr.send();
xhr.onload = function() {
  var blob = xhr.response;
  var audio = document.createElement('audio');
  audio.onload = function(e) {
    window.URL.revokeObjectURL(audio.src);
  }
  audio.src = window.URL.createObjectURL(blob);
  audio.controls = 'controls';
  document.body.appendChild(audio);
}
```

注意: HTML5直接支持的音频文件类型只有: `mp3`、`ogg`、`wav` 



### 处理视频类型文件

```js
var url = 'http://localhost/test.mp4';
window.URL = window.URL || window.webkitURL;

var xhr = new XMLHttpRequest();
xhr.open('GET', url, true);
xhr.responseType = 'blob';
xhr.send();
xhr.onload = function() {
  var blob = xhr.response;
  var video = document.createElement('video');
  video.onload = function(e) {
    window.URL.revokeObjectURL(video.src);
  }
  video.src = window.URL.createObjectURL(blob);
  video.controls = 'controls';
  document.body.appendChild(video);
}
```

注意: HTML5直接支持的视频文件类型只有: `MP4`、`WebM`、`Ogg`  



## responseType新类型 - ArrayBuffer

[`ArrayBuffer`](https://developer.mozilla.org/en/JavaScript_typed_arrays/ArrayBuffer) 是二进制数据通用的固定长度容器。如果您需要原始数据的通用缓冲区，ArrayBuffer 就非常好用，但是它真正强大的功能是让您使用 JavaScript 类型数组创建底层数据的“视图”。实际上，可以通过单个`ArrayBuffer` 来源创建多个视图。

```js
var url = 'http://localhost/test.png';
var xhr = new XMLHttpRequest();
xhr.open('GET', url, true);
xhr.responseType = 'arraybuffer';
xhr.send();
xhr.onload = function() {
  console.log(xhr.response);
  var blob = new Blob([xhr.response]);
  console.log(blob)
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0049.png)

将`responseType`设置为`arraybuffer`即可返回`arraybuffer`类型的数据。

可以使用`Blob`构造函数将`arraybuffer`的数据转化为`Blob`，注意得加上中括号`[]`。



### 类型化数组

类型化数组(Typed Arrays)是JavaScript中新出现的一个概念，专为访问原始的二进制数据而生。

类型数组的类型有：

| 名称               | 大小 (以字节为单位) | 说明       |
| ---------------- | ----------- | -------- |
| **Int8Array**    | 1           | 8位有符号整数  |
| **Uint8Array**   | 1           | 8位无符号整数  |
| **Int16Array**   | 2           | 16位有符号整数 |
| **Uint16Array**  | 2           | 16位无符号整数 |
| **Int32Array**   | 4           | 32位有符号整数 |
| **Uint32Array**  | 4           | 32位无符号整数 |
| **Float32Array** | 4           | 32位浮点数   |
| **Float64Array** | 8           | 64位浮点数   |

本质上，类型化数组和ArrayBuffer是一样的。不过一个可读写（脱掉buffer限制），一个当数据源的命。

```js
var url = 'http://localhost/test.png';
var xhr = new XMLHttpRequest();
xhr.open('GET', url, true);
xhr.responseType = 'arraybuffer';
xhr.send();
xhr.onload = function() {
  console.log(this.response)
  var uInt8Array = new Uint8Array(this.response)
  console.log(uInt8Array)
  var int8Array = new Int8Array(this.response)
  console.log(int8Array)
  var uInt16Array = new Uint16Array(this.response)
  console.log(uInt16Array)
  var int16Array = new Int16Array(this.response)
  console.log(int16Array)
  var int32Array = new Int32Array(this.response)
  console.log(int32Array)
  var float32Array = new Float32Array(this.response)
  console.log(float32Array)
  var float64Array = new Float64Array(this.response)
  console.log(float64Array)
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0050.png)



## responseType新类型 - Document

当请求一段具有格式的`html`或`xml`格式的数据时，可以指定`responseType`为`Document`，解析为一段文档。如:

```js
var url = 'http://localhost/test.html';
var xhr = new XMLHttpRequest();
xhr.open('GET', url, true);
xhr.responseType = 'document';
xhr.send();
xhr.onreadystatechange = function () {
  console.log(xhr)
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      console.log(xhr.response)
    }
  }
}
```

可以看到控制台打印:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0039.png)

注意到此时`xhr`多了一个`responseXML`属性:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0040.png)

其中比较有用的属性有:

### responseXML.all

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0041.png)

### responseXML.doctype

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0043.png)

### responseXML.documentElement

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0044.png)

其他可能有用的属性还包括:

返回`HTMLCollection`类型的属性: 

```
childNodes、children、images、forms、links、plugins、scripts、embeds
```

返回`StyleSheetList`类型的属性:

```
styleSheets
```

返回`NodeList`类型的属性:

```
childNodes
```

其他属性慢慢探索。



# CORS跨域请求

以前都是使用`jsonp`动态生成`script`标签进行跨域资源获取，而`XMLHttpRequest2`可以使用`CORS`(跨域资源共享，Cross-Origin Resource Sharing)实现跨域请求。

通常在后端语言(如: PHP)或者是服务器(如: Apache)配置文件中都可以设置`CORS`。

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
```

## 在PHP中设置

```php
header("Access-Control-Allow-Origin:*"); 
```

## 在Apache中设置

```xml
<Directory />
	Options FollowSymLinks 
	AllowOverride None 
	Header set Access-Control-Allow-Origin *
</Directory>
```

`service httpd restart`重启服务

## 在Nginx中设置

```nginx
#
# Wide-open CORS config for nginx
#
location / {
     if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        #
        # Custom headers and headers various browsers *should* be OK with but aren't
        #
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
        #
        # Tell client that this pre-flight info is valid for 20 days
        #
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain charset=UTF-8';
        add_header 'Content-Length' 0;
        return 204;
     }
     if ($request_method = 'POST') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
     }
     if ($request_method = 'GET') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
     }
}
```

`service nginx restart`重启服务



# 参考资料

[XMLHttpRequest2 新技巧](http://blog.csdn.net/hills/article/details/41246939) 

[XML DOM - XMLHttpRequest 对象](http://www.w3school.com.cn/xmldom/dom_http.asp) 

[史上最全的AJAX之XMLHttpRequest方法和属性详解](http://blog.csdn.net/huang_cai_yuan/article/details/54881407) 

[理解DOMString、Document、FormData、Blob、File、ArrayBuffer数据类型](http://www.zhangxinxu.com/wordpress/2013/10/understand-domstring-document-formdata-blob-file-arraybuffer/) 

[Ajax之不可或缺的setRequestHeader()](http://www.zhangxinxu.com/wordpress/2013/10/understand-domstring-document-formdata-blob-file-arraybuffer/) 

[XMLHTTP中setRequestHeader参数问题](http://blog.csdn.net/iamduoluo/article/details/7215639) 

[readyState与status](http://www.cnblogs.com/zhaojing-0504/p/5971330.html) 

[FormData 对象的使用](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects) 

[Window.URL](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/URL) 

[HTTP访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS) 

[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html) 

[CORS 跨域 实现思路及相关解决方案](http://www.cnblogs.com/sloong/p/cors.html) 

[Apache2 同源策略解决方案 - 配置 CORS](http://blog.csdn.net/lzhlzz/article/details/53302863) 

[HTML5 Blob与ArrayBuffer、TypeArray和字符串String之间转换](http://www.cnblogs.com/tianma3798/p/5834598.html) 