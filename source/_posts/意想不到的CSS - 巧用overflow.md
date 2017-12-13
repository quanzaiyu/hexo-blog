---
title: 意想不到的CSS - 巧用overflow
categories:
  - 前端开发
  - 意想不到的CSS
tags:
  - CSS3
---



在微信中看到一篇文章，关于overflow的，以前主要用overflow也就控制一些元素的溢出处理。而文章作者却将overflow玩出新高度，自己回头也做了一些demo，感觉特别有意思。



先看看效果:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0.gif)

# 如何实现

先看看一段简单的代码

```css
.overflower {
  line-height: 1.5em;
  display: inline-block;
  padding: 0 1em;
  border: 1px solid #000;
  box-sizing: border-box;
  max-width: 100%;
  height: 1.5em;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.overflower-long {
  display: inline;
}
.overflower-short {
  display: none;
}
@supports (flex-wrap: wrap) {
  .overflower {
    display: inline-flex;
    flex-wrap: wrap;
  }
  .overflower-short {
    display: block;
    overflow: hidden;
    width: 0;
    text-overflow: ellipsis;
    flex-grow: 1;
  }
  .overflower-long {
    flex-basis: 100%;
  }
}
```

```html
<span class="overflower">
  <!-- 放置短文本的容器 -->
  <span class="overflower-short" aria-hidden="true" title="Some long text that could become shorter">
    Short text here is.</span>
  <!-- 放置长文本的容器 -->
  <span class="overflower-long">Some long text that could become shorter.</span>
</span>
```

效果为慢慢缩短容器的宽度，可以看到显示不同的长度的文本。



# 原理

- 使用@supports做了一个渐进增强的处理，如果浏览器支持flex-wrap属性，那么将使用Flexbox的一些属性的特性，比如容器overflower为inline-flex容器，然后配合flex-grow:1和text-overflow:ellipsis来处理短文本样式，对于长文本，将flex-basis设置为100%
- 另外需要给容器一个固定的高度。所以设置height的值，同时为了文本能垂直居中，再设置line-height的值和height等同
- 对于不支持flex-wrap的浏览器，在overflower也就是最外面的容器中，通过text-overflow:ellipsis和white-space来控制文本，当然这个时候短文本就不显示了



# 缺点

虽然这种方法让我们实现了灵活的`overflow`，效果是更让人感觉很爽，但却让我们的HTML变得冗余。对于追求HTML简洁干净的人来说简直是不可忍受的事。



更炫的Demo在[codepen](https://codepen.io/airen/pen/vWbyJd) 




# 参考资料

[wechat: 灵活的 overflow](http://mp.weixin.qq.com/s/cs7S8ybzkEJuIV_UOQ9bGg) 

[w3cplus: 灵活的overflow](https://www.w3cplus.com/css/flexible-overflow.html) 