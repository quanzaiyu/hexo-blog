---
title: Canvas学习笔记01 - 基础
categories:
  - 前端开发
  - Canvas学习笔记
tags:
  - Canvas
---



# Canvas简介

`<canvas>` 是 `HTML5` 新增的，一个可以使用脚本(通常为`JavaScript`)在其中绘制图像的 `HTML` 元素。它可以用来制作照片集或者制作简单(也不是那么简单)的动画，甚至可以进行实时视频处理和渲染。



# 创建Canvas

`<canvas>`会创建一个固定大小的画布，会公开一个或多个 **渲染上下文**(画笔)，使用 **渲染上下文**来绘制和处理要展示的内容。

```html
<canvas id="canvas" width="200" height="200"></canvas>
```

其中`width`和`height`并不是指`canvas`的真正尺寸，而是指`canvas`的精度，即将整个画布平分为200*200个像素点，真正指定`canvas`尺寸大小可以由CSS指定

```css
#canvas {
  width: 500px;
  height: 500px;
}
```

通过js操作canvas的**渲染上下文**(Thre Rending Context)画笔实现绘制

```js
let canvas = document.getElementById('canvas') // 获取canvas元素 -> HTMLCanvasElement
let ctx = canvas.getContext('2d') // 获取到canvas上下文(画笔) -> CanvasRenderingContext2D
```



# 栅格和坐标空间

上面说了，canvas的属性width和height将canvas平均分布200*200个像素点，左上顶点坐标为(0, 0)，右下顶点为(200, 200)，如图所示:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/Canvas_default_grid.png)



# 绘制形状

canvas只支持一种基本形状——矩形，所有其它形状都是通过一个或多个路径组合而成，甚至是基本的矩形也可以通过路径组合成。 更多的图形可以使用第三方插件 [draw2d.js](http://www.draw2d.org/draw2d/) 完成。

## 矩形

canvas提供三个绘制矩形的方法:

1. `fillRect(x, y, width, height)` 绘制一个填充的矩形
2. `strokeRect(x, y, width, height)` 绘制一个矩形的边框
3. `clearRect(x, y, widh, height)` 清除指定的矩形区域，然后这块区域会变的完全透明。(可以理解为一块矩形橡皮擦)

参数说明:

- `x, y`：矩形的左上角的坐标。
- `width, height`：绘制的矩形的宽和高。

填充的默认颜色为黑色

如:

```js
ctx.fillRect(10, 10, 100, 50);
ctx.strokeRect(10, 70, 100, 50);
ctx.clearRect(15, 15, 50, 25);
```



# 绘制路径

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。

使用路径绘制图形一般需要以下步骤：

1. 创建路径起始点
2. 调用绘制方法去绘制出路径
3. 把路径封闭
4. 一旦路径生成，通过描边或填充路径区域来渲染图形。

canvas绘制路径相关的API:

1. `beginPath()` 新建一条路径，路径一旦创建成功，图形绘制命令被指向到路径上生成路径
2. `moveTo(x, y)` 把画笔移动到指定的坐标`(x, y)`。相当于设置路径的起始点坐标。(可以理解为将画笔悬空移动)
3. `lineTo(x, y)` 将画笔以画线的形式移动到另一点坐标`(x, y)`。(可以理解为让画笔在画纸上移动)
4. `closePath()` 闭合路径之后，图形绘制命令又重新指向到上下文中
5. `stroke()` 通过线条来绘制图形轮廓
6. `fill()` 通过填充路径的内容区域生成实心的图形

如: 通过路径绘制一个矩形

```js
ctx.beginPath();
ctx.moveTo(50, 50);
ctx.lineTo(50, 100);
ctx.lineTo(100, 100);
ctx.lineTo(100, 50);
ctx.closePath();
ctx.fill();
```



# 添加颜色和样式

## 颜色

给绘制的图形上色，可以使用以下API:

1. `fillStyle = color` 设置图形的填充颜色

2. `strokeStyle = color` 设置图形轮廓的颜色

3. `globalAlpha = transparencyValue`  这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0。

    `globalAlpha` 属性在需要绘制大量拥有相同透明度的图形时候相当高效。不过，个人认为使用`rgba()`设置透明度更加好一些。

注意:

1. `color` 可以是表示 `css` 颜色值的字符串、渐变对象或者图案对象。
2. 默认情况下，线条和填充颜色都是黑色。
3. 一旦您设置了 `strokeStyle` 或者 `fillStyle` 的值，那么这个新值就会成为新绘制的图形的默认值。如果你要给每个图形上不同的颜色，需要重新设置 `fillStyle`。

如: 

```js
for (let i = 0; i < 100; i++){
  for (let j = 0; j < 100; j++){
    for (let k = 0; k < 100; k++) {
      ctx.fillStyle = 'rgb(' +
        Math.floor(255 - 20 * i) + ',' +
        Math.floor(255 - 20 * j) + ',' +
        Math.floor(255 - 20 * k) + ')';
      ctx.fillRect(j * 10, i * 10, 10, 10);
    }
  }
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0012.png)

```js
function randomInt(from, to){
  return parseInt(Math.random() * (to - from + 1) + from);
}
for (var i = 0; i < 6; i++){
  for (var j = 0; j < 6; j++){
    ctx.strokeStyle = `rgb(${randomInt(0, 255)},${randomInt(0, 255)},${randomInt(0, 255)})`;
    ctx.strokeRect(j * 50, i * 50, 40, 40);
  }
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0013.png)

## 样式

1. `lineWidth = value` 线宽。只能是正值。默认是`1.0`
2. `lineCap = type` 线条末端样式，允许的值有:

   1. `butt`：线段末端以方形结束
   2. `round`：线段末端以圆形结束
   3. `square`：线段末端以方形结束，但是增加了一个宽度和线段相同，高度是线段厚度一半的矩形区域。
3. `lineJoin = type` 同一个path内，设定线条与线条间接合处的样式。
   1. `round` 通过填充一个额外的，圆心在相连部分末端的扇形，绘制拐角的形状。 圆角的半径是线段的宽度。
   2. `bevel` 在相连部分的末端填充一个额外的以三角形为底的区域， 每个部分都有各自独立的矩形拐角。
   3. `miter` (默认) 通过延伸相连部分的外边缘，使其相交于一点，形成一个额外的菱形区域。
4. 设置虚线样式: `setLineDash` 方法接受一个数组，来指定线段与间隙的交替；`lineDashOffset`属性设置起始偏移量。

如: 设置线条末端样式

```js
ctx.beginPath(); 
ctx.moveTo(10, 10); 
ctx.lineTo(100, 10); 
ctx.lineWidth = 10; 
ctx.lineCap = 'round'
ctx.stroke();
```

如: 设置线条结合处样式

```js
var lineJoin = ['round', 'bevel', 'miter'];
ctx.lineWidth = 20;
for (var i = 0; i < lineJoin.length; i++){
  ctx.lineJoin = lineJoin[i];
  ctx.beginPath();
  ctx.moveTo(50, 50 + i * 50);
  ctx.lineTo(100, 100 + i * 50);
  ctx.lineTo(150, 50 + i * 50);
  ctx.lineTo(200, 100 + i * 50);
  ctx.lineTo(250, 50 + i * 50);
  ctx.stroke();
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0015.png)

如: 设置虚线样式

```js
ctx.setLineDash([20, 5, 10, 5]);  // [实线长度, 间隙长度]
ctx.lineDashOffset = 10;
ctx.strokeRect(50, 50, 100, 100);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0014.png)



# 绘制文字

canvas 提供了两种方法来渲染文本:

1. `fillText(text, x, y [, maxWidth])` 在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.
2. `strokeText(text, x, y [, maxWidth])` 在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.

```js
let text = 'Hello canvas!'
ctx.font = "20px sans-serif"
ctx.fillText(text, 50, 50)
ctx.strokeText(text, 50, 100)
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0016.png)

## 文本样式

1. `font = value` 当前我们用来绘制文本的样式。这个字符串使用和 `CSS font`属性相同的语法. 默认的字体是 `10px sans-serif`。
2. `textAlign = value` 文本对齐选项. 可选的值包括：`start`, `end`, `left`, `right` or `center`. 默认值是 `start`。
3. `textBaseline = value` 基线对齐选项，可选的值包括：`top`, `hanging`, `middle`, `alphabetic`, `ideographic`, `bottom`。默认值是 `alphabetic。`
4. `direction = value` 文本方向。可能的值包括：`ltr`, `rtl`, `inherit`。默认值是 `inherit。`



# 绘制图片

使用`drawImage`绘制图像

有以下三种使用方法:

1. `context.drawImage(img,x,y)`
2. `context.drawImage(img,x,y,width,height)`
3. `context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height)`



- 第一参数`img`可以是一个`Image()`的实例，也可以是一个`<img>`的Dom元素。

- `sx, sy` 必选，为绘制图像的顶点坐标。
- `sWidth, sHeight` 可选，为图片缩放大小。

```js
var img = new Image();   // 创建img元素
img.onload = function(){
  ctx.drawImage(img, 20, 20, 150, 100)
}
img.src = 'city.jpg'; // 设置图片源地址
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0017.png)

如果除`img`外有8个参数:

- 前4个是定义图像源的切片位置和大小。
- 后4个则是定义切片的目标显示位置和大小。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0018.jpg) ![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0020.png)




# 参考资料

 [学习HTML5 Canvas这一篇文章就够了](http://blog.csdn.net/u012468376/article/details/73350998) 

[HTML5 Canvas 学习](http://www.jianshu.com/p/14164d222f0b) 

[HTML5之Canvas 2D入门2 - Canvas绘制图形](http://blog.csdn.net/pyx6119822/article/details/52384637) 