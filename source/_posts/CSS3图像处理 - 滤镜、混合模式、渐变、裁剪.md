---
title: CSS3图像处理 - 滤镜、混合模式、渐变、裁剪
categories:
  - 前端开发
tags:
  - CSS3
---



在以前，图像处理通常是使用PS进行美化，现在，CSS3也能做到这些。



本文针对svg图像做处理，原图为:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter01.png)

素材地址: [前景图](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/firefox-logo.svg) [背景图](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/bg.jpg) 



# 滤镜 Filter

## 模糊 blur

blur用来给图像设置高斯模糊。参数值设定高斯函数的标准差，或者是屏幕上以多少像素融在一起，这个值设置为百分比除外的css长度值，默认是0为原图，值越大越模糊，当值大于图片的宽高最大值时就什么都没了。

```css
filter: blur(5px);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter02.png)


## 透明度 opacity

opacity会调整图片的透明度，这个和filter中的opacity效果是一样哒，但是并不是一个属性呢，因为他们是可以叠加使用的。

opacity只能接受小数，filter:opactiy()既可以接受小数也可以接受百分比，值越小越透明。

```css
filter: opacity(.5);
opacity: .5; /*注意，可以和filter属性的opacity重叠，这里相当于是 .5 * .5 = .25 */
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter03.png)

## 灰度 grayscale

grayscale为图片设置灰度，当值为100%时就成为完全的灰度图片了。

```css
filter: grayscale(100%);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter04.png)

## 对比度 contrast

contrast的参数接受百分比形式的数值也接受小数形式的，值为0 的时候是整个图片都是灰黑色的，为1时是原图，值越大对比度越大，默认值为1。

```css
filter: contrast(200%);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter05.png)

## 饱和度 saturate

饱度可以理解为图像的彩色程度，当为0%时就是一张灰度图了。

```css
filter: saturate(50%);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter10.png)

## 亮度 brightness

亮度值默认值为1，大于1变亮，小于1变暗。

```css
filter: brightness(50%);
```





## 阴影 drop-shadow

添加阴影效果可不只有text-shadow和box-shadow哦，text-shadow是为文字添加阴影，box-shadow给一个元素添加阴影，drop-shadow在图片是非png或svg情况下和box-shadow有些相似，然而png或svg图片才是她大放异彩的地方。

```css
filter: drop-shadow(20px 20px 30px red);
```



![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter06.png)

## 老照片 sepia

使用sepia(深褐色)可以渲染出一张怀旧的照片。参数可以是小数也可以是百分比。

```css
filter: sepia(100%);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter07.png)

## 色相 hue-rotate

hue-rotate 参数是一个角度值，他会接受这个值并把图片中的颜色的色相做对应的旋转。补基础: [css里颜色的那些事儿(合法颜色值)](http://www.cnblogs.com/sanweimiao/p/6307650.html) 

```css
filter: hue-rotate(45deg);
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter08.png)



## 反转 invert

invert会把图片上的所有颜色进行反转，如果值为100%，就做了个相机底片。

```css
filter: invert(100%);
```



![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter09.png)



# 混合模式 blend-mode

混合模式的值的对应效果可以完全类比PS中图层模式效果，他们的对应关系是：

```
1. normal 正常模式
2. multiply 正片叠底模式
3. screen 滤色模式
4. overlay 叠加模式
5. darken 变暗模式
6. lighten 变亮模式
7. color-burn 颜色加深模式
8. hard-light 强光模式
9. soft-light 柔光模式
10. difference 差值模式
11. exclusion 排除模式
12. hue 色相模式
13. saturation 饱和度模式
14. color 颜色模式
15. luminosity 亮度模式
```

## mix-blend-mode

mix-blend-mode主要作用是把目标元素和其下方的背景元素混合。

```html
<div class="mode">
  <img src="./bg.jpg" alt="" class='bg'>
  <img src="./firefox-logo.svg" alt="" class='content'>
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
  width: 100%;
}
.content {
  width: 50%;
  height: 50%;
  position: absolute;
  mix-blend-mode: luminosity;
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter11.png)

## background-blend-mode

background-blend-mode是作用于background-image和background-color的。并且是写在一个background属性后面的图片。

```css
.bg {
  width: 100%;
  height: 200px;
  background-size: 100% 100%;
  background-image: url(./bg.jpg);
  background-color: blue;
  background-blend-mode: screen;
}
```

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter12.png)





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

[MDN: filter](https://developer.mozilla.org/en-US/docs/Web/CSS/filter) 

[MDN: Blend Mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/blend-mode) 

[MDN: mix-blend-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/mix-blend-mode) 

[MDN: background-blend-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/background-blend-mode) 

[MDN: linear-gradient](https://developer.mozilla.org/en-US/docs/Web/CSS/linear-gradient) 

[MDN: repeating-linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/repeating-linear-gradient) 

[Runoob: fliter](http://www.runoob.com/cssref/css3-pr-filter.html) 

[详解CSS中的合成和混合模式-Blend modes](http://www.htmleaf.com/ziliaoku/qianduanjiaocheng/201503171537.html) 

[如何用 CSS 修出好看的照片](http://mp.weixin.qq.com/s/JF8jAWWklsvBarUR54al2g) 

[CSS混合模式](https://www.w3cplus.com/css3/basics-css-blend-modes.html) 

[CSS3中的mix-blend-mode和background-blend-mode](http://blog.csdn.net/u014291497/article/details/77620973) 

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