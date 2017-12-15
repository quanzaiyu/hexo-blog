---
title: 意想不到的CSS - CSS3图像处理 - 渐变、裁剪
categories:
  - 前端开发
  - 意想不到的CSS
tags:
  - CSS3
---



# 渐变 gradient

## 线性渐变 linear-gradient

### 渐变线

在渐变容器中，穿过容器中心点和颜色停止点连接在一起的线称为渐变线。在一个笛卡尔坐标系中，y轴正方向为0deg(to top)，x轴正方向为90deg(to right)，y轴负方向为180deg(to bottom)，x轴负方向为270deg(to left)

```css
background-image: linear-gradient(0deg, red, white);
```

![](https://www.w3cplus.com/sites/default/files/blogs/2017/1703/gradient-1.png)

### 渐变角度

角度简写对应的角度值

| to              | deg    | turn     |
| --------------- | ------ | -------- |
| to top          | 0deg   | 1turn    |
| to right        | 90deg  | .25turn  |
| to bottom       | 180deg | .5turn   |
| to left         | 270deg | .75turn  |
| to top right    | 45deg  | .125turn |
| to bottom right | 135deg | .375turn |
| to bottom left  | 225deg | .625turn |
| to top left     | 315deg | .875turn |

### 渐变线长度

![](https://www.w3cplus.com/sites/default/files/blogs/2017/1703/gradient-8.png)

通常，浏览器会计算渐变线的长度，以覆盖整个填充容器，计算公式为: 

```
abs(W * sin(A)) + abs(H * cos(A))
```

其中: `W`是渐变容器的宽度，`H`是渐变容器的高度，`A`是渐变角度

### 渐变色节点（Color stops）

渐变色的每一个颜色可以这样定义：

```
<color> [<percentage> | <length>]
```

默认情况下是均分

![](https://www.w3cplus.com/sites/default/files/blogs/2017/1703/gradient-9.png)

在样式中可以指定各项颜色的线长:

```css
background-image: linear-gradient(.125turn, red 30%, pink 70%, white);
```

其中，red从起始位置到整个渐变线的30%位置，pink从起始位置到整个渐变线的70%位置，white未指定则从0%到100%位置。

使用此特性，可以做出各种精美的纹理:

![](https://www.w3cplus.com/sites/default/files/blogs/2017/1703/gradient-10.png)

![](https://www.w3cplus.com/sites/default/files/blogs/2017/1703/gradient-11.png)

![](https://www.w3cplus.com/sites/default/files/blogs/2017/1703/gradient-13.png)

注意，如果后面的颜色占比小于前面颜色占比，则会出现明显的分界线:

![](https://www.w3cplus.com/sites/default/files/blogs/2017/1703/gradient-17.png)



### 渐变透明度(Transparency)

透明度还支持透明渐变，这对于制作一些特殊的效果是相当有用的。

```css
background-image: linear-gradient(.125turn, rgba(255,0,0,.5) 30%, rgba(255,255,0,.5) 70%, white), url(./bg.jpg);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter13.png)

### 可累加的线性渐变背景图

线性渐变支持累加效果，不过注意为前面的颜色设置透明度，否则是看不到效果的。

```css
background: linear-gradient(.125turn, rgba(255,0,0,.6) 30%, rgba(255,255,0,.4) 60%),
      linear-gradient(green 40%, blue 80%, white);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient11.png)

### 可重复的线性渐变

使用可重复的线性渐变可轻松做出各种好看的纹理、进度条等。

```css
background: repeating-linear-gradient(135deg, #000 0, #000 .25em, #0092b7 0, #0092b7 .5em);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient16.png)



## 径向渐变 radial-gradient

径向渐变中，默认渐变线为容器对角线，如果是外接一个椭圆，则为长轴与短轴所接矩形的对角线，如果是一个完美的正方形，则为外接圆的半径。

径向渐变方式主要由`<size>`、`<position>`、`<shape>`这三个参数影响，分别控制椭圆的圆心、形状和大小。

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/radial-gradient-ray.png)

其渐变范围（渐变结束线）示意如下图，默认既不是按照宽度来的，也不是按照高度来的，是按照最远边角距离作为渐变结束线的:

```css
background: radial-gradient(yellow,red);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/circle-gradient-end.png)

### 指定渐变形状

| 形状      | 说明   |
| ------- | ---- |
| circle  | 圆形   |
| ellipse | 椭圆形  |

#### 圆形 circle

```css
background: radial-gradient(circle, yellow, red);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient01.png)

#### 椭圆形 ellipse

```css
background: radial-gradient(ellipse, yellow, red);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient07.png)

### 指定渐变起始点位置

起始点位置可以有如下设置

| 起始点位置           | 说明    |
| --------------- | ----- |
| at top          | 位于上方  |
| at right        | 位于右方  |
| at bottom       | 位于下方  |
| at left         | 位于左方  |
| at top right    | 位于右上方 |
| at bottom right | 位于右下方 |
| at bottom left  | 位于左下方 |
| at top left     | 位于左上方 |

如:

```css
background: radial-gradient(300px circle at top left,yellow,red);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient04.png)

### 指定渐变终止点位置

`radial-gradient`径向渐变支持4个关键字可以指定渐变终止点位置:

| 关键字               | 描述                  |
| ----------------- | ------------------- |
| `closest-side`    | 渐变中心距离容器最近的边作为终止位置。 |
| `closest-corner`  | 渐变中心距离容器最近的角作为终止位置。 |
| `farthest-side`   | 渐变中心距离容器最远的边作为终止位置。 |
| `farthest-corner` | 渐变中心距离容器最远的角作为终止位置。 |

```css
background: radial-gradient(farthest-side circle,yellow,red);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient05.png)

也可以使用长度单位指定渐变线的长度:

```css
background: radial-gradient(100px circle,yellow,red);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient03.png)

如果是椭圆，可以指定长轴和短轴的长度:

```css
background: radial-gradient(300px 100px ellipse,yellow,red);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient08.png)

### 指定渐变颜色节点

后面的参数可以指定渐变颜色节点

```css
background: radial-gradient(closest-side circle, yellow, orange, red, white);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient06.png)

指定颜色节点位置

```css
background: radial-gradient(300px 100px ellipse,yellow 10%,red);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient09.png)



### 可累加的径向渐变背景图

```css
background: radial-gradient(50px 100px ellipse,
    transparent 40px, yellow 41px, red 49px, transparent 50px),
  radial-gradient(30px circle, red, red 29px, transparent 30px);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient10.png)

### 渐变背景尺寸控制

可以通过`background-size`属性控制背景的尺寸大小。注意不要设置`background-repeat: no-repeat; `

```css
width: 100%;
height: 200px;
background: radial-gradient(5px circle, transparent 4px, yellow 5px, red 6px, transparent 7px),
	radial-gradient(3px circle, red, red 3px, transparent 4px);
background-color: rgba(255,255,0,.3);
background-size: 25px 50px;
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient12.png)

### 可重复的径向渐变

```css
background: repeating-radial-gradient(#ace, #ace 5px, #f96 5px, #f96 10px);
border-radius: 50%;
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient18.png)



## 一些简单的应用

### 制作水纹特效

```css
.radial-gradient {
  width: 200px; height: 100px;
  background: red;
  position: relative;
}
.radial-gradient:after {
  content: '';
  position: absolute;
  height: 10px;
  left:0 ; right: 0;
  bottom: -10px;
  background: radial-gradient(20px 15px ellipse at top, red 10px, transparent 11px);
  background-size: 20px 10px;
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient13.png)

### 制作唱片特效

来自 [https://codepen.io/thebabydino/embed/HjJlL](https://codepen.io/thebabydino/embed/HjJlL) 

```css
.radial-gradient {
  position: relative;
  width: 262px; height: 262px;
  border-radius: 50%;
  background: linear-gradient(30deg, transparent 40%, rgba(42, 41, 40, .85) 40%) no-repeat 100% 0, linear-gradient(60deg, rgba(42, 41, 40, .85) 60%, transparent 60%) no-repeat 0 100%, repeating-radial-gradient(#2a2928, #2a2928 4px, #ada9a0 5px, #2a2928 6px);
  background-size: 50% 100%, 100% 50%, 100% 100%;
}
.radial-gradient:after {
  position: absolute;
  top: 50%; left: 50%;
  margin: -35px;
  border: solid 1px #d9a388;
  width: 68px; height: 68px;
  border-radius: 50%;
  box-shadow: 0 0 0 4px #da5b33, inset 0 0 0 27px #da5b33;
  background: #b5ac9a;
  content: '';
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient14.png)

### 制作镂空特效

```css
.radial-gradient {
  width: 100px; height: 100px;
  border: 50px solid;
  border-image: radial-gradient(circle, transparent 50px, yellow 51px, red) 50 stretch;
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient15.png)

### 制作调色板

```css
.board {
  width: 200px;
  height: 200px;
  background: 
    linear-gradient(36deg, #272b66 42.34%, transparent 42.34%),
    linear-gradient(72deg, #2d559f 75.48%, transparent 75.48%),
    linear-gradient(-36deg, #9ac147 42.34%, transparent 42.34%) 100% 0,
    linear-gradient(-72deg, #639b47 75.48%, transparent 75.48%) 100% 0, 
    linear-gradient(36deg, transparent 57.66%, #e1e23b 57.66%) 100% 100%,
    linear-gradient(72deg, transparent 24.52%, #f7941e 24.52%) 100% 100%,
    linear-gradient(-36deg, transparent 57.66%, #662a6c 57.66%) 0 100%,
    linear-gradient(-72deg, transparent 24.52%, #9a1d34 24.52%) 0 100%, 
    #43a1cd linear-gradient(#ba3e2e, #ba3e2e) 50% 100%;
  background-repeat: no-repeat;
  background-size: 50% 50%;
  border-radius: 50%;
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/gradient17.png)



# 剪裁 clip

### clip (不推荐使用)

`clip`的优势：`clip` 属性是CSS中出现的第一种剪裁。`clip`是运行在浏览器中的，它可能会一直有效。而浏览器对它的支持是非常强大的：几乎是有史以来的每一个浏览器。

`clip`的劣势:`clip` 只对绝对定位的元素有效，`clip` 只能用于矩形，即`rect()`函数。

格式:

```css
clip: rect(top, right, bottom, left);
```

如:

```html
<div class="mode">
  <div class="bg"></div>
</div>
```

```css
.mode {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}
.bg {
  width: 300px;
  height: 300px;
  background: repeating-radial-gradient(#ace, #ace 5px, #f96 5px, #f96 10px);
  border-radius: 50%;
  position: absolute;
  clip: rect(0,200px,200px,0);
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/clip02.png)

参考demo: http://www.zhangxinxu.com/study/201103/css-rect-demo.html



![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/clip01.png)

### clip-path

参考 demo: http://bennettfeely.com/clippy/






# 参考资料

[MDN: linear-gradient](https://developer.mozilla.org/en-US/docs/Web/CSS/linear-gradient) 

[MDN: repeating-linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/repeating-linear-gradient) 

[CSS3 经典教程系列：CSS3 线性渐变（linear-gradient）](http://www.cnblogs.com/lhb25/archive/2013/02/17/css3-linear-gradient.html) 

[你真的理解CSS的linear-gradient？](https://www.w3cplus.com/css3/do-you-really-understand-css-linear-gradients.html) 

[CSS3:background新增特性详解](http://blog.csdn.net/crper/article/details/51145696) 

[再说CSS3渐变——径向渐变](https://www.w3cplus.com/css3/new-css3-radial-gradient.html) 

[CSS3 Gradient](https://www.w3cplus.com/content/css3-gradient) 

[深入理解CSS径向渐变radial-gradient](http://www.cnblogs.com/xiaohuochai/p/5383285.html) 

[CSS3 radial-gradient径向渐变语法及辅助理解案例10则](http://www.zhangxinxu.com/wordpress/2017/11/css3-radial-gradient-syntax-example/) 

[为什么要使用repeating-linear-gradient](https://www.w3cplus.com/css3/why-do-we-have-repeating-linear-gradient-anyway.html) 

[Dig deep into CSS linear gradients](https://hugogiraudel.com/2013/02/04/css-gradients/#a-few-things-about-linear-gradients) 

[CSS clip:rect矩形剪裁功能及一些应用介绍](http://www.zhangxinxu.com/wordpress/2011/04/css-clip-rect/) 

[CSS中的剪裁和遮罩](https://www.w3cplus.com/css3/clipping-masking-css.html) 

[CSS裁剪clip](http://www.cnblogs.com/xiaohuochai/p/5285752.html) 