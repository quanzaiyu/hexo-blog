---
title: Canvas学习笔记03 - 渐变
categories:
  - 前端开发
  - Canvas学习笔记
tags:
  - Canvas
---



# 线性渐变

使用`ctx.createLinearGradient(x1, y1, x2, y2)`可以创建一个线性渐变，线性渐变会从第一个点(x1, y1)扩展到第二个点(x2, y2)，即定义了渐变的线长与方向。

如:

```js
var x1 = 0;
var y1 = 0;
var x2 = 100;
var y2 = 0;
var linearGradient1 = ctx.createLinearGradient(x1, y1, x2, y2);
linearGradient1.addColorStop(0, 'rgb(255, 0, 0)');
linearGradient1.addColorStop(0.5, 'rgb(0, 0, 255');
linearGradient1.addColorStop(1, 'rgb(0, 0, 0)');
ctx.fillStyle = linearGradient1

ctx.fillRect(10, 10, 100, 50);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0029.png)

使用`addColorStop`可以添加一个颜色节点

- 第一个参数是0-1之间的一个数值，这个数值指定该颜色进入渐变多长的距离
- 第二个参数是颜色值




# 径向渐变

径向渐变是一种圆形的颜色扩展模式，颜色从圆心位置开始向外辐射。

一个径向渐变于两个圆形来定义。每一个圆都有一个圆心和一条半径。

使用`ctx.createRadialGradient(x1, y1, r1, x2, y2, r2)`可以创建一个径向渐变，(x1, y1, r1)和(x2, y2, r2)分别为两个圆的圆心坐标和半径。

```js
var x1 = 100;   // 第一个圆圆心的X坐标
var y1 = 100;   // 第一个圆圆心的Y坐标
var r1 = 30;    // 第一个圆的半径
var x2 = 100;   // 第二个圆圆心的X坐标
var y2 = 100;   // 第二个圆圆心的Y坐标
var r2 = 100;   // 第二个圆的半径
var radialGradient1 = ctx.createRadialGradient(x1, y1, r1, x2, y2, r2);
radialGradient1.addColorStop(0, 'rgb(0, 0, 255)');
radialGradient1.addColorStop(1, 'rgb(0, 255, 0)');
ctx.fillStyle = radialGradient1

ctx.fillRect(10, 10, 200, 200);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0030.png)

`addColorStop`的用法同线性渐变。

如果两个圆形的圆心位置相同，那么径向渐变将是一个完整的圆形。如果两个圆的圆心位置不相同，那么径向渐变看起来就像是一个探照灯发出的光线。如:

```js
var x1 = 100;   // 第一个圆圆心的X坐标
var y1 = 100;   // 第一个圆圆心的Y坐标
var r1 = 30;    // 第一个圆的半径
var x2 = 150;   // 第二个圆圆心的X坐标
var y2 = 120;   // 第二个圆圆心的Y坐标
var r2 = 100;   // 第二个圆的半径
var radialGradient1 = ctx.createRadialGradient(x1, y1, r1, x2, y2, r2);
radialGradient1.addColorStop(0, 'rgb(0, 0, 255)');
radialGradient1.addColorStop(1, 'rgb(0, 255, 0)');
ctx.fillStyle = radialGradient1

ctx.fillRect(10, 10, 200, 200);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0031.png)



# 参考资料

[PHP中文网: HTML5 Canvas：绘制渐变色](http://www.php.cn/html5-tutorial-35205.html) 