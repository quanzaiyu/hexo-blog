---
title: Canvas学习笔记06 - 绘制一些简单的图像
categories:
  - 前端开发
  - Canvas学习笔记
tags:
  - Canvas
---

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas03.png)

```js
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  ctx.fillRect(0,0,300,300);
  for (var i=0;i<3;i++) {
    for (var j=0;j<3;j++) {
      ctx.save();
      ctx.strokeStyle = "#9CFF00";
      ctx.translate(50+j*100,50+i*100);
      drawSpirograph(ctx,20*(j+2)/(j+1),-8*(i+3)/(i+1),10);
      ctx.restore();
    }
  }
}
function drawSpirograph(ctx,R,r,O){
  var x1 = R-O;
  var y1 = 0;
  var i    = 1;
  ctx.beginPath();
  ctx.moveTo(x1,y1);
  do {
    if (i>20000) break;
    var x2 = (R+r)*Math.cos(i*Math.PI/72) - (r+O)*Math.cos(((R+r)/r)*(i*Math.PI/72))
    var y2 = (R+r)*Math.sin(i*Math.PI/72) - (r+O)*Math.sin(((R+r)/r)*(i*Math.PI/72))
    ctx.lineTo(x2,y2);
    x1 = x2;
    y1 = y2;
    i++;
  } while (x2 != R-O && y2 != 0 );
  ctx.stroke();
}
draw();
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas04.png)

```js
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  ctx.translate(75,75);  // 移动原点到(75,75)处

  for (var i=1;i<6;i++){ // 里往外画5圈圆
    ctx.save(); // 先保存状态
    ctx.fillStyle = 'rgb('+ (50*i) +','+ (255-20*i) +',' + (30*i) + ')'; // 圆的颜色

    // 下面实现效果是：通过旋转画板，在x轴上画圆。这样的好处是方便计算，所有圆在x轴上实现，通过旋转画板来画所有圆。
    for (var j=0; j<i*6; j++) {
      ctx.rotate(Math.PI*2/(i*6)); // 顺时针旋转Math.PI*2/(i*6)度
      ctx.beginPath();
      ctx.arc(0,i*12.5,5,0,Math.PI*2,true); // 在(0,12.5*i)处画圆，半径为5px，画360度。
      ctx.fill();
    }

    ctx.restore(); // 还原到保存前的状态
  }
}
draw();
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas17.png)

```js
var sin = Math.sin(Math.PI/6);
var cos = Math.cos(Math.PI/6);
ctx.translate(100, 100);
var c = 0;
for (var i=0; i <= 12; i++) {
  c = Math.floor(255 / 12 * i);
  ctx.fillStyle = "rgb(" + c + "," + c + "," + c + ")";
  ctx.fillRect(0, 0, 100, 10);
  ctx.transform(cos, sin, -sin, cos, 0, 0);
}
```






# 参考资料

[canvas 变形记——移动、旋转、缩放、变形](http://blog.csdn.net/u014181418/article/details/51726843) 