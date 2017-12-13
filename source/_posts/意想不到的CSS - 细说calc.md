---
title: 意想不到的CSS - 细说calc
categories:
  - 前端开发
  - 意想不到的CSS
tags:
  - CSS3
---



在很早以前，就知道CSS3有一个计算函数`calc()`，但一直没有深入了解。今天在别人的博客上偶然看到了关于`calc`的详细介绍，于是燃起了深入了解此特性的想法。



# 什么是Calc

`calc`是英文单词`calculate`的缩写，是css3的一个新增的功能，用来指定元素的长度。比如说，你可以使用`calc()`给元素的border、margin、pading、font-size和width等属性设置动态值。为何说是动态值呢?因为我们使用的表达式来得到的值。不过calc()最大的好处就是用在流体布局上，可以通过calc()计算得到元素的宽高。



# 如何使用

`calc()`使用通用的数学运算规则，但是也提供更智能的功能：

1. 使用“+”、“-”、“*” 和 “/”四则运算；
2. 可以使用%、px、em、rem等单位；
3. 可以混合使用各种单位进行计算；
4. 表达式中有“+”和“-”时，其前后必须要有空格，如"widht: calc(12%+5em)"这种没有空格的写法是错误的；
5. 表达式中有“*”和“/”时，其前后可以没有空格，但建议留有空格。

使用起来其实很简单，比如:

```css
.box {
  width: calc(50% + 2em);
  height: calc(100% - 2em);
  background-color: #f00;
}
```



# 兼容性

查询结果来自: https://caniuse.com/#search=calc

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/calc.png)

总体上，兼容性还不错，本人目前主要针对移动端做开发，还过得去。




# 参考资料

[CSS3的calc()使用](https://www.w3cplus.com/css3/how-to-use-css3-calc-function.html) 