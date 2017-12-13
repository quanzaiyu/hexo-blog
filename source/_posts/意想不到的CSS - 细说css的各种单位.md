---
title: 意想不到的CSS - 细说css的各种单位
categories:
  - 前端开发
  - 意想不到的CSS
tags:
  - CSS3
---



最近在各种网页上陆续看到出现各种以前都没怎么了解过的单位，譬如vw、vh、pc、turn等等，在网上查阅了很多资料，打算深入一番了解这些单位的作用。



# CSS长度单位

| 单位   | 含义                            |
| ---- | ----------------------------- |
| em   | 相对于父元素的字体大小                   |
| ex   | 相对于小写字母"x"的高度                 |
| gd   | 一般用在东亚字体排版上，这个与英文并无关系         |
| rem  | 相对于根元素字体大小                    |
| vw   | 相对于视窗的宽度：视窗宽度是100vw           |
| vh   | 相对于视窗的高度：视窗高度是100vh           |
| vm   | 相对于视窗的宽度或高度，取决于哪个更小           |
| ch   | 相对于0尺寸                        |
| px   | 相对于屏幕分辨率而不是视窗大小：通常为1个点或1/72英寸 |
| in   | inch, 表英寸                     |
| cm   | centimeter, 表厘米               |
| mm   | millimeter, 表毫米               |
| pt   | 1/72英寸                        |
| pc   | 12点活字，或1/12点                  |
| %    | 相对于父元素。正常情况下是通过属性定义自身或其他元素    |

其中常用的有px、%、em、rem，至于其他的，不常用，之前也没怎么深入理解。这里详细理解一下以下几个单位:

## vw、vh、vm

这几个单位都是跟视窗有关，vw为视窗宽度，总宽度为100vw，vh为视窗高度，总高度为100vh，vm为视窗宽高的较小值。

所以，1vw相当于视窗宽度的1%，1vh相当于视窗高度的1%。

### 何为视窗?

“视窗”所指为浏览器内部的可视区域大小，即`window.innerWidth` * `window.innerHeight`的大小。参见鑫空间的[demo](http://www.zhangxinxu.com/study/201209/vw-vh-to-pixel.html) 

### 兼容性

查了下兼容性，总体支持还不错:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/caniuse01.png)



# CSS时间频率单位

| 单位   | 含义                |
| ---- | ----------------- |
| ms   | milliseconds, 毫秒数 |
| s    | seconds, 秒数       |
| Hz   | Hertz, 赫兹         |
| kHz  | kilohertz, 千赫     |

都比较常见。



# CSS角度单位

| 单位   | 含义          |
| ---- | ----------- |
| deg  | degrees, 角度 |
| grad | grads, 百分度  |
| rad  | radians, 弧度 |
| turn | turns, 圈数   |

说下turn吧，其实1turn就是360deg，.5turn就是180deg，有的时候写起来比较方便，不过像45deg这种角度还是不要写成turn了。




# 参考资料

[鑫空间: CSS/CSS3长度、时间、频率、角度单位大全](http://www.zhangxinxu.com/wordpress/2011/03/css-css3-unit-units/) 

[鑫空间: 视区相关单位vw, vh..简介以及可实际应用场景](http://www.zhangxinxu.com/wordpress/2011/03/css-css3-unit-units/) 