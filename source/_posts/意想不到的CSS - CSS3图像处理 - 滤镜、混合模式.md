---
title: 意想不到的CSS - CSS3图像处理 - 滤镜、混合模式
categories:
  - 前端开发
  - 意想不到的CSS
tags:
  - CSS3
---



在以前，图像处理通常是使用PS进行美化，现在，CSS3也能做到这些。



本文针对svg图像做处理，原图为:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/filter01.png)

素材地址: [前景图](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/firefox-logo.svg) [背景图](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/bg.jpg) 



# 滤镜 Filter

在我的GitHub上做了一个简单的[demo](https://github.com/quanzaiyu/css3-filter-test)，用于测试filter

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




# 参考资料

[MDN: filter](https://developer.mozilla.org/en-US/docs/Web/CSS/filter) 

[MDN: Blend Mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/blend-mode) 

[MDN: mix-blend-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/mix-blend-mode) 

[MDN: background-blend-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/background-blend-mode) 

[Runoob: fliter](http://www.runoob.com/cssref/css3-pr-filter.html) 

[详解CSS中的合成和混合模式-Blend modes](http://www.htmleaf.com/ziliaoku/qianduanjiaocheng/201503171537.html) 

[如何用 CSS 修出好看的照片](http://mp.weixin.qq.com/s/JF8jAWWklsvBarUR54al2g) 

[CSS混合模式](https://www.w3cplus.com/css3/basics-css-blend-modes.html) 

[CSS3中的mix-blend-mode和background-blend-mode](http://blog.csdn.net/u014291497/article/details/77620973)  
