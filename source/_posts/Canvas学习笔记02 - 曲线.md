---
title: Canvas学习笔记02 - 曲线
categories:
  - 前端开发
  - Canvas学习笔记
tags:
  - Canvas
---



# 绘制圆弧

## arc

`arc(x, y, r, startAngle, endAngle, anticlockwise)`

- 以`(x, y)`为圆心，以`r`为半径，从 `startAngle`弧度开始到`endAngle`弧度结束，注意: 单位为弧度。
- `anticlosewise`是布尔值，`true`表示逆时针，`false`表示顺时针。(默认是顺时针)
- 0弧度为在一个笛卡尔坐标系中的x轴正方向
- 通常使用`Math.PI`进行弧度运算，一个`Math.PI`就是`180deg`
- `radians=(Math.PI/180)*degrees`   // 角度转换成弧度1

如:

```js
ctx.beginPath();
ctx.arc(50, 50, 40, 0, Math.PI / 2, false);
ctx.stroke();
```

可以看到从x轴正方向顺时针绘制出了半径为40的 1/4 圆弧。



## arcTo

`arcTo(x1, y1, x2, y2, radius)`

根据给定的控制点和半径画一段圆弧，最后再以直线连接两个控制点。

```js
ctx.beginPath();
ctx.moveTo(50, 50);
//参数1、2：控制点1坐标   参数3、4：控制点2坐标  参数5：圆弧半径
ctx.arcTo(200, 50, 200, 200, 50);
ctx.lineTo(200, 200)
ctx.stroke();
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0021.jpg)

可以理解为: 绘制的弧形是由两条切线所决定。

 第 1 条切线：起始点和控制点1决定的直线。

 第 2 条切线：控制点1 和控制点2决定的直线。

圆弧半径可以回想一下css中`border-radius`的实现。



# 贝塞尔曲线

贝塞尔曲线(Bézier curve)，又称贝兹曲线或贝济埃曲线，是应用于二维图形应用程序的数学曲线。

 一般的矢量图形软件通过它来精确画出曲线，贝兹曲线由线段与节点组成，节点是可拖动的支点，线段像可伸缩的皮筋，我们在绘图工具上看到的钢笔工具就是来做这种矢量曲线的。

## 原理动画

一次贝塞尔曲线:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0022.jpg) 

二次贝塞尔曲线:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0023.jpg) 

 ![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0024.jpg) 

三次贝塞尔曲线:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0025.jpg) 

 ![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0026.jpg)



## 绘制贝塞尔曲线

### 绘制二次贝塞尔曲线

`quadraticCurveTo(cp1x, cp1y, x, y)`

参数说明：

- 参数1和2：控制点坐标
- 参数3和4：结束点坐标

```js
ctx.beginPath();
var bX = 10, bY = 160; // 起始点
ctx.moveTo(bX, bY);
var cX = 40, cY = 100;  // 控制点
var toX = 180, toY = 180; // 结束点
// 绘制二次贝塞尔曲线
ctx.quadraticCurveTo(cX, cY, toX, toY);
ctx.stroke();

ctx.beginPath();
ctx.rect(bX, bY, 10, 10);
ctx.rect(cX, cY, 10, 10);
ctx.rect(toX, toY, 10, 10);
ctx.fill();
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0027.png)

为了方便理解，将起始点、控制点、结束点都用实心矩形标出。



### 绘制三次贝塞尔曲线

`bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`：

参数说明：

- 参数1和2：控制点1的坐标
- 参数3和4：控制点2的坐标
- 参数5和6：结束点的坐标

```js
ctx.beginPath();
var bX = 10, bY = 160; // 起始点
ctx.moveTo(bX, bY);
var cX1 = 20, cY1 = 50;  // 控制点1
var cX2 = 60, cY2 = 150;  // 控制点2
var toX = 180, toY = 180; // 结束点
// 绘制二次贝塞尔曲线
ctx.bezierCurveTo(cX1, cY1, cX2, cY2, toX, toY);
ctx.stroke();

ctx.beginPath();
ctx.rect(bX, bY, 10, 10);
ctx.rect(cX1, cY1, 10, 10);
ctx.rect(cX2, cY2, 10, 10);
ctx.rect(toX, toY, 10, 10);
ctx.fill();
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0028.png)




# 参考资料

 [学习HTML5 Canvas这一篇文章就够了](http://blog.csdn.net/u012468376/article/details/73350998) 

[贝塞尔曲线与CSS3动画、SVG和canvas的基情](http://www.zhangxinxu.com/wordpress/2013/08/%E8%B4%9D%E5%A1%9E%E5%B0%94%E6%9B%B2%E7%BA%BF-cubic-bezier-css3%E5%8A%A8%E7%94%BB-svg-canvas/) 