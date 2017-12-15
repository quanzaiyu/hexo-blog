---
title: Canvas学习笔记07 - 动画
categories:
  - 前端开发
  - Canvas学习笔记
tags:
  - Canvas
---



# 动画的基本步骤

1. **清空canvas**

   再绘制每一帧动画之前，需要清空所有。清空所有最简单的做法就是`clearRect()`方法

2. **保存canvas状态**

   如果在绘制的过程中会更改`canvas`的状态(颜色、移动了坐标原点等),又在绘制每一帧时都是原始状态的话，则最好保存下`canvas`的状态

3. **绘制动画图形**

   这一步才是真正的绘制动画帧

4. **恢复canvas状态**

   如果你前面保存了`canvas`状态，则应该在绘制完成一帧之后恢复`canvas`状态。



# 控制动画

我们可用通过`canvas`的方法或者自定义的方法把图像会知道到`canvas`上。正常情况，我们能看到绘制的结果是在脚本执行结束之后。例如，我们不可能在一个 `for` 循环内部完成动画。

也就是，为了执行动画，我们需要一些可以定时执行重绘的方法。

一般用到下面三个方法：

1. `setInterval()`
2. `setTimeout()`
3. `requestAnimationFrame()`



# 案例

## 太阳系

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0032.png)

```js
let sun;
let earth;
let moon;
let ctx;
function init(){
  sun = new Image();
  earth = new Image();
  moon = new Image();
  sun.src = "http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/sun.png";
  earth.src = "http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/earth.png";
  moon.src = "http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/moon.png";

  let canvas = document.querySelector("#solar");
  ctx = canvas.getContext("2d");

  sun.onload = function (){
    draw()
  }

}
init();
function draw(){
  ctx.clearRect(0, 0, 300, 300); //清空所有的内容
  /*绘制 太阳*/
  ctx.drawImage(sun, 130, 130, 30, 30);

  ctx.save();
  ctx.translate(150, 150);

  //绘制earth轨道
  ctx.beginPath();
  ctx.strokeStyle = "rgba(0,255,0,0.6)";
  ctx.arc(0, 0, 100, 0, 2 * Math.PI)
  ctx.stroke()

  let time = new Date();
  //绘制地球
  ctx.rotate(2 * Math.PI / 60 * time.getSeconds() + 2 * Math.PI / 60000 * time.getMilliseconds())
  ctx.translate(100, 0);
  ctx.drawImage(earth, -12, -12, 20, 20)

  //绘制月球轨道
  ctx.beginPath();
  ctx.strokeStyle = "rgba(255,0,0,.6)";
  ctx.arc(0, 0, 40, 0, 2 * Math.PI);
  ctx.stroke();

  //绘制月球
  ctx.rotate(2 * Math.PI / 6 * time.getSeconds() + 2 * Math.PI / 6000 * time.getMilliseconds());
  ctx.translate(40, 0);
  ctx.drawImage(moon, -3.5, -3.5, 10, 10);
  ctx.restore();

  requestAnimationFrame(draw);
}
```



## 时钟

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0033.png)

```js
init();

function init(){
  let canvas = document.querySelector("#clock");
  let ctx = canvas.getContext("2d");
  draw(ctx);
}

function draw(ctx){
  requestAnimationFrame(function step(){
    drawDial(ctx); //绘制表盘
    drawAllHands(ctx); //绘制时分秒针
    requestAnimationFrame(step);
  });
}
/*绘制时分秒针*/
function drawAllHands(ctx){
  let time = new Date();

  let s = time.getSeconds();
  let m = time.getMinutes();
  let h = time.getHours();

  let pi = Math.PI;
  // 1° = π / 180 ≈ 0.01745 rad
  // 1rad = 180 / π = 57.30°
  let secondAngle = pi / 180 * 6 * s;  //计算出来s针的弧度
  let minuteAngle = pi / 180 * 6 * m + secondAngle / 60;  //计算出来分针的弧度
  let hourAngle = pi / 180 * 30 * h + minuteAngle / 12;  //计算出来时针的弧度

  drawHand(hourAngle, 60, 6, "red", ctx);  //绘制时针
  drawHand(minuteAngle, 106, 4, "green", ctx);  //绘制分针
  drawHand(secondAngle, 129, 2, "blue", ctx);  //绘制秒针
}
/*绘制时针、或分针、或秒针
     * 参数1：要绘制的针的角度
     * 参数2：要绘制的针的长度
     * 参数3：要绘制的针的宽度
     * 参数4：要绘制的针的颜色
     * 参数4：ctx
     * */
function drawHand(angle, len, width, color, ctx){
  ctx.save();
  ctx.translate(150, 150);
  ctx.rotate(-Math.PI / 2 + angle);  //旋转坐标轴。 x轴就是针的角度
  ctx.beginPath();
  ctx.moveTo(-4, 0);
  ctx.lineTo(len, 0);  // 沿着x轴绘制针
  ctx.lineWidth = width;
  ctx.strokeStyle = color;
  ctx.lineCap = "round";
  ctx.stroke();
  ctx.closePath();
  ctx.restore();
}

/*绘制表盘*/
function drawDial(ctx){
  let pi = Math.PI;

  ctx.clearRect(0, 0, 300, 300); //清除所有内容
  ctx.save();

  ctx.translate(150, 150); //一定坐标原点到原来的中心
  ctx.beginPath();
  ctx.arc(0, 0, 148, 0, 2 * pi); //绘制圆周
  ctx.stroke();
  ctx.closePath();

  for (let i = 0; i < 60; i++){//绘制刻度。
    ctx.save();
    ctx.rotate(-pi / 2 + i * pi / 30);  //旋转坐标轴。坐标轴x的正方形从 向上开始算起
    ctx.beginPath();
    ctx.moveTo(110, 0);
    ctx.lineTo(140, 0);
    ctx.lineWidth = i % 5 ? 2 : 4;
    ctx.strokeStyle = i % 5 ? "blue" : "red";
    ctx.stroke();
    ctx.closePath();
    ctx.restore();
  }
  ctx.restore();
}
```




# 参考资料

[MDN: 基本的动画](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Basic_animations) 

[学习HTML5 Canvas这一篇文章就够了](http://blog.csdn.net/u012468376/article/details/73350998) 

[8个经典炫酷的HTML5 Canvas动画欣赏](http://www.html5tricks.com/8-html5-canvas-animation-view.html) 

[html5 tricks: html5-canvas](http://www.html5tricks.com/html5-canvas-3d-cube-wave.html)  

[分享8款令人惊叹的HTML5 Canvas动画特效](http://www.html5tricks.com/8-html5-canvas-animation.html) 