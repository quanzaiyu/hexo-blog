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








# 参考资料

[HTML5 canvas drawImage() 方法](http://www.w3school.com.cn/tags/canvas_drawimage.asp) 