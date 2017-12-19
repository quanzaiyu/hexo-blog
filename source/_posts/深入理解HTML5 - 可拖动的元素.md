---
title: 深入理解HTML5 - 可拖动的元素
categories:
  - 前端开发
  - 深入理解HTML5
tags:
  - 拖动
---



示例代码:

```html
<!DOCTYPE html>
<html class="no-js">

<head>
  <meta charset="utf-8">
  <title>HTML5-draggable(拖放)</title>
  <style type="text/css">
    #div1,
    #div2 {
      float: left;
      width: 100px;
      height: 35px;
      margin: 10px;
      padding: 10px;
      border: 1px solid #aaaaaa;
    }
  </style>
  <script type="text/javascript">
    /*
     * 虽然已经设定了img元素可被拖动，但是浏览器默认地，无法将数据/元素放置到其他元素中。
     * 如果有需要设置某些元素可接受被拖动元素，则要阻止它的默认行为，
     * 这要通过设置该接收元素的ondragover 事件，调用event.preventDefault() 方法
     */
    function allowDrop(ev) {
      ev.preventDefault(); //阻止默认行为

      //ev.target.id
      //此处ev.target是接收元素，通过事件被绑定在哪个元素即可区分
    }

    /*
     * 当该img元素被拖动时，会触发一个ondragstart 事件，该事件调用了一个方法drag(event)。
     */
    function drag(ev) {
      //ev.dataTransfer.setData() 方法设置被拖数据的数据类型（Text）和值（被拖元素id），
      //该方法将被拖动元素的id存储到事件的dataTransfer对象内，ev.dataTransfer.getData()可将该元素取出。
      //此处ev.target是被拖动元素
      ev.dataTransfer.setData("Text", ev.target.id);
    }

    /*
     * 当被拖元素移动到接收元素，
     * 松开鼠标时（即被拖元素放置在接收元素内时）会出发ondrop事件
     */
    function drop(ev) {
      ev.preventDefault(); //阻止默认行为
      var data = ev.dataTransfer.getData("Text"); //将被拖动元素id取出
      ev.target.appendChild(document.getElementById(data)); //将被拖动元素添加到接收元素尾部
    }
  </script>
</head>

<body>

  <div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)">
    <!--为了使元素可拖动，把 draggable 属性设置为 true-->
    <img src="http://www.w3school.com.cn/i/w3school_logo_black.gif" draggable="true" ondragstart="drag(event)" id="drag1" />
  </div>

  <div id="div2" ondrop="drop(event)" ondragover="allowDrop(event)"></div>

</body>

</html>
```



# 参考资料

[HTML5-draggable(拖放)](http://www.cnblogs.com/blog-leo/p/4457697.html) 

[HTML5 drag & drop 拖拽与拖放简介](http://www.zhangxinxu.com/wordpress/2011/02/html5-drag-drop-%E6%8B%96%E6%8B%BD%E4%B8%8E%E6%8B%96%E6%94%BE%E7%AE%80%E4%BB%8B/) 