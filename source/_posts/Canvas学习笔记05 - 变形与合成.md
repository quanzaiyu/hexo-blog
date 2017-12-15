---
title: Canvas学习笔记05 - 变形与合成
categories:
  - 前端开发
  - Canvas学习笔记
tags:
  - Canvas
---



# translate

`translate(x, y)`

 用来移动 `canvas` 的**原点**到指定的位置(坐标变换)

 `translate`方法接受两个参数:

- `x` 是左右偏移量
- `y` 是上下偏移量

在做变形之前先保存状态是一个良好的习惯。大多数情况下，调用 `restore` 方法比手动恢复原先的状态要简单得多。又如果你是在一个循环中做位移但没有保存和恢复`canvas` 的状态，很可能到最后会发现怎么有些东西不见了，那是因为它很可能已经超出 `canvas` 范围以外了。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas01.jpg)

```js
ctx.save(); //保存坐原点平移之前的状态
ctx.translate(100, 100);
ctx.strokeRect(0, 0, 100, 100)
ctx.restore(); //恢复到最初状态
ctx.translate(200, 200);
ctx.fillRect(0, 0, 100, 100)
```



# rotate

`rotate(angle)`

 旋转坐标轴， 旋转的中心是**坐标原点**。

 这个方法只接受一个参数：旋转的角度(angle)，它是**顺时针方向**的，以弧度为单位的值。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas02.jpg)

```js
ctx.save()
ctx.translate(100, 100)
ctx.fillRect(0,0,50,100)
ctx.fillStyle = '#f00'
ctx.rotate(Math.PI/2)
ctx.fillRect(0,0,50,100)
ctx.fillStyle = '#0f0'
ctx.rotate(Math.PI/2)
ctx.fillRect(0,0,50,100)
ctx.fillStyle = '#00f'
ctx.rotate(Math.PI/2)
ctx.fillRect(0,0,50,100)
ctx.restore()
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0034.png)

```js
ctx.save()

ctx.translate(120, 120)
ctx.lineWidth = 10

ctx.beginPath()
ctx.strokeStyle = '#f00'
ctx.moveTo(20, 20)
ctx.lineTo(100,100)
ctx.stroke()
ctx.closePath()

ctx.rotate(Math.PI/2)
ctx.beginPath()
ctx.strokeStyle = '#0f0'
ctx.moveTo(20, 20)
ctx.lineTo(100,100)
ctx.stroke()
ctx.closePath()

ctx.rotate(Math.PI/2)
ctx.beginPath()
ctx.strokeStyle = '#00f'
ctx.moveTo(20, 20)
ctx.lineTo(100,100)
ctx.stroke()
ctx.closePath()

ctx.rotate(Math.PI/2)
ctx.beginPath()
ctx.strokeStyle = '#ff0'
ctx.moveTo(20, 20)
ctx.lineTo(100,100)
ctx.stroke()
ctx.closePath()

ctx.restore()
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0035.png)



# scale

`scale(x, y)`

 我们用它来增减图形在 `canvas` 中的像素数目，对形状，位图进行缩小或者放大，。

 `scale`方法接受两个参数。`x,y`分别是横轴和纵轴的缩放因子，它们都必须是正值。值比 1.0 小表示缩 小，比 1.0 大则表示放大，值为 1.0 时什么效果都没有。

举例说，如果`canvas` 的 1 单位就是 1 个像素。我们设置缩放因子是 0.5，1 个单位就变成对应 0.5 个像素，这样绘制出来的形状就会是原先的一半。同理，设置为 2.0 时，1 个单位就对应变成了 2 像素，绘制的结果就是图形放大了 2 倍。

```js
ctx.save()

ctx.translate(120, 120)

ctx.strokeStyle = '#f00'
ctx.strokeRect(0,0,50,100)

ctx.scale(.5, .5)
ctx.fillStyle = '#0f0'
ctx.fillRect(0,0,50,100)

ctx.restore()
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0036.png)



# transforms

transforms 直接对变形矩阵作修改。

transform(m11, m12, m21, m22, dx, dy) 这个方法必须将当前的变形矩阵乘上下面的矩阵：

```
m11 m21 dx
m12 m22 dy
0   0   1
```

这个方法必须重置当前的变形矩阵为单位矩阵，然后以相同的参数调用 transform 方法。如果任意一个参数是无限大，那么变形矩阵也必须被标记为无限大，否则会抛出异常。

`a (m11)` Horizontal scaling.

`b (m12)` Horizontal skewing.

`c (m21)` Vertical skewing.

`d (m22)` Vertical scaling.

`e (dx)` Horizontal moving.

`f (dy)` Vertical moving.

```js
ctx.transform(1, 0, 1, 1, 0, 0);
ctx.fillRect(0, 0, 100, 100);
```



# 合成

```js
globalCompositeOperation = type
```

例子

```js
ctx.fillStyle = "blue";
ctx.fillRect(0, 0, 200, 200);

ctx.globalCompositeOperation = "source-over"; //全局合成操作
ctx.fillStyle = "red";
ctx.fillRect(100, 100, 200, 200);
```

`type`是下面 13 种字符串值之一：

1. **source-over** (default) 这是默认设置，新图像会覆盖在原有图像。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas05.png)

2. **source-in** 仅仅会出现新图像与原来图像重叠的部分，其他区域都变成透明的。(包括其他的老图像区域也会透明)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas06.png)

3. **source-out** 仅仅显示新图像与老图像没有重叠的部分，其余部分全部透明。(老图像也不显示)



![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas07.png)

4. **source-atop** 新图像仅仅显示与老图像重叠区域。老图像仍然可以显示。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas08.png)

5. **destination-over** 新图像会在老图像的下面。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas09.png)

6. **destination-in** 仅仅新老图像重叠部分的老图像被显示，其他区域全部透明。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas10.png)

7. **destination-out** 仅仅老图像与新图像没有重叠的部分。 注意显示的是老图像的部分区域。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas11.png)

8. **destination-atop** 老图像仅仅仅仅显示重叠部分，新图像会显示在老图像的下面。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas12.png)

9. **lighter** 新老图像都显示，但是重叠区域的颜色做加处理。

如:

```css
blue: #0000ff
red: #ff0000
所以重叠部分的颜色：#ff00ff
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas14.png)

10. **darken** 保留重叠部分最黑的像素。(每个颜色位进行比较，得到最小的)。

如:

```css
blue: #0000ff
red: #ff0000
所以重叠部分的颜色：#000000
```



![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas13.png)

11. **lighten** 保证重叠部分最量的像素。(每个颜色位进行比较，得到最大的)。

如:

```css
blue: #0000ff
red: #ff0000
所以重叠部分的颜色：#ff00ff
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas14.png)

12. **xor** 重叠部分会变成透明。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas15.png)

13. **copy** 只有新图像会被保留，其余的全部被清除(边透明)。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/canvas16.png)




# 参考资料

[学习HTML5 Canvas这一篇文章就够了](http://blog.csdn.net/u012468376/article/details/73350998) 