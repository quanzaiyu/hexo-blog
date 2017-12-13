---
title: 深入理解JavaScript - Blob、Base64及其转化
categories:
  - 前端开发
  - 深入理解JavaScript
tags:
  - Blob
  - Base64
---
# 前言

之所以会写这篇文章，是由于前段时间研究JS的语音播放，当时想着使用H5自带的`audio`进行处理，然而上传的语音文件却是`.amr`格式的(个人不太熟悉这个格式，感觉很罕见)，使用`audio`只能处理`mp3`、`wav`、`ogg`三种格式，`amr`显然对H5并不友好。

处理这个格式花费了大量功夫，想了很多办法，比如前端上传前类型转换、后端上传后类型转换、寻找可以播放`amr` 格式的插件等等，折腾了好久。由于是用`Vue`进行开发的，之前想用`MUI`集成的`H5+`进行播放也不能实现(必须打包成APP才行，需要HBuilder的基座支持)。当时找了一个名叫`amrnb.js`(amr牛逼啊，哈哈，开玩笑的，全称为: Adaptive Multi Rate-Narrow Band Speech Codec，用C++进行开发的)的插件，的确可以做到播放和转化`amr`格式，但是文件太大，光是压缩后的文件都接近500k，果断放弃了。

最后想到了将多媒体文件转化为`Base64`进行使用，但如何将服务器中的文件转化为`Base64`又成了问题，`FileReader`可以实现转化，但是JS浏览器端并不能对本地文件系统(或者远程URI进行操作)，只能从`input[type=file]`的输入框读取，`node`虽然可以对文件系统进行操作，但却不能实现在浏览器的转化。郁闷了好几天，后来想到直接通过`ajax`向服务器请求多媒体文件不就可以了，查了些资料就开始操作了。为什么会提到`Blob`呢?是由于请求下来的格式可以指定为此格式，进行操作比较容易。 

## 什么是Blob

BLOB (binary large object)，二进制大对象，是一个可以存储二进制文件的容器。在计算机中，BLOB常常是数据库中用来存储二进制文件的字段类型。([百度百科](https://baike.baidu.com/item/blob/543419?fr=aladdin))

Blob在JS中是一种对象类型。

## 什么是Base64

Base64是网络上最常见的用于传输8Bit字节码的编码方式之一，Base64就是一种基于64个可打印字符来表示二进制数据的方法。Base64编码是从二进制到字符的过程，可用于在HTTP环境下传递较长的标识信息。可查看RFC2045～RFC2049，上面有MIME的详细规范。([百度百科](https://baike.baidu.com/item/base64/8545775?fr=aladdin))



# 数据获取与格式转化

## 从服务器端获取Blob格式的资源

```js
let fetchBlob = (url, callback) => {
  var xhr = new XMLHttpRequest()
  xhr.open('GET', url)
  xhr.responseType = 'blob'
  xhr.onload = function () {
    callback(this.response)
  }
  xhr.onerror = function () {
    alert('Failed to fetch ' + url)
  }
  xhr.send()
}
```

调用:

```js
fetchBlob('http://localhost/test.amr', () => {
  // 回调处理
})
```

## Blob对象转化为Base64

```js
let readBlobAsDataURL = (blob, callback) => {
  var reader = new FileReader()
  reader.onload = function (e) {
    callback(e.target.result)
  }
  reader.readAsDataURL(blob)
}
```

File对象转换为dataURL与Blob一致。

## base64转换为Blob对象

```js
function dataURLtoBlob(dataurl) {
    var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
    while(n--){
        u8arr[n] = bstr.charCodeAt(n);
    }
    return new Blob([u8arr], {type:mime});
}
//test:
var blob = dataURLtoBlob('data:text/plain;base64,YWFhYWFhYQ==');
```

## canvas转换为base64

```js
var dataurl = canvas.toDataURL('image/png');
var dataurl2 = canvas.toDataURL('image/jpeg', 0.8);
```

## base64图片数据绘制到canvas

先构造Image对象，src为dataURL，图片onload之后绘制到canvas

```js
var img = new Image();
img.onload = function(){
    canvas.drawImage(img);
};
img.src = dataurl;
```



# AMR的处理

至于如何播放amr格式，我做了一个Vue的插件，已上传到个人OSS，有兴趣的童鞋可以自行下载研究。[下载地址](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/res/pVioce.zip) 

使用的话很简单，将插件注册一下就能直接使用:

```js
import pVioce from './pVioce'
Vue.component('pVioce', pVioce)
```

组件中使用(支持mp3, wav, ogg, amr):

```js
<pVioce :src="http://localhost/test.amr"></pVioce>
<pVioce :src="http://localhost/test.mp3"></pVioce>
```



# 参考资料

[AMRNB 与 AMRWB 语音编码标准技术的对比研究](http://www.lxway.com/122204946.htm) 

[[JS进阶] JS 之Blob 对象类型](http://blog.csdn.net/oscar999/article/details/36373183) 

[DataURL与File,Blob,canvas对象之间的互相转换的Javascript](http://blog.csdn.net/cuixiping/article/details/45932793) (精)