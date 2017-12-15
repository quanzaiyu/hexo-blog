---
title: Canvas学习笔记04 - 图像视频处理
categories:
  - 前端开发
  - Canvas学习笔记
tags:
  - Canvas
---



# 使用canvas捕捉视频图像

使用`drawImage`甚至可以接受一个`video`的dom作为参数:

```html
<video src="http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/mov_bbb.mp4" controls></video>
<canvas id="canvas" width="200" height="200"></canvas>
<script>
  let canvas = document.getElementById('canvas')
  let video = document.querySelector('video')
  let ctx = canvas.getContext('2d')
  let i = undefined
  video.addEventListener('play', function() {
    i = window.setInterval(function() {
      ctx.drawImage(video, 0, 0, 270, 135)
    }, 20);
  }, false);
  video.addEventListener('pause', function() {
    window.clearInterval(i);
  }, false);
  video.addEventListener('ended', function() {
    clearInterval(i);
  }, false);
</script>
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0019.png)

# 剪裁路径

`clip()` 把已经创建的路径转换成裁剪路径。

裁剪路径的作用是遮罩。只显示裁剪路径内的区域，裁剪路径外的区域会被隐藏。

注意：`clip()`只能遮罩在这个方法调用之后绘制的图像，如果是`clip()`方法调用之前绘制的图像，则无法实现遮罩。

```js
ctx.beginPath();
ctx.arc(20,20, 100, 0, Math.PI * 2);
ctx.fillStyle = "pink";
ctx.save();

ctx.clip();
ctx.fillRect(20, 20, 100,100);

ctx.restore()
ctx.fillRect(100, 100, 100,100);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas18.png)




# 参考资料

[HTML5 canvas drawImage() 方法](http://www.w3school.com.cn/tags/canvas_drawimage.asp) 