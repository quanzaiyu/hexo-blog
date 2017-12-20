---
title: 深入理解HTML5 - 可拖动的元素
categories:
  - 前端开发
  - 深入理解HTML5
tags:
  - 拖动
---



# 拖动

## 拖拽和释放

拖拽(`Drag`)指的是鼠标点击源对象后一直移动对象不松手，一但松手即释放(`Drop`)了



## 源对象与目标对象

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0077.png)

源对象：指的是我们鼠标点击的一个事物，这里可以是一张图片，一个DIV，一段文本等等。

目标对象：指的是我们拖动源对象后移动到一块区域，源对象可以进入这个区域，可以在这个区域上方悬停(未松手)，可以释松手释放将源对象放置此处(已松手)，也可以悬停后离开该区域。



# 拖拽API

## 被拖动的源对象可以触发的事件

1. `ondragstart`：源对象开始被拖动
2. `ondrag`：源对象被拖动过程中(鼠标可能在移动也可能未移动)
3. `ondragend`：源对象被拖动结束

## 拖动源对象可以进入到上方的目标对象可以触发的事件

1. `ondragenter`：目标对象被源对象拖动着进入
2. `ondragover`：目标对象被源对象拖动着悬停在上方
3. `ondragleave`：源对象拖动着离开了目标对象
4. `ondrop`：源对象拖动着在目标对象上方释放/松手

## 传递数据

HTML5为所有的拖动相关事件提供了一个新的属性：

`e.dataTransfer` 

  功能：数据传递对象，用于在源对象和目标对象的事件间传递数据

### 源对象上的事件处理中保存数据

`e.dataTransfer.setData(k,  v);`     // k-v必须都是string类型

### 目标对象上的事件处理中读取数据

`var v = e.dataTransfer.getData(k);`



# 案例

## 在几个盒子之间拖动元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    .container {
      display: flex;
      justify-content: space-around;
    }
    .box {
      width: 200px;
      height: 200px;
      border: 1px dashed #f00;
      position: relative;
    }
    .target {
      width: 100px;
      height: 100px;
      background: #000;
      position: absolute;
      left: 1em;
      top: 1em;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="box">
      <div class="target" id="target" draggable="true"></div>
    </div>
    <div class="box"></div>
    <div class="box"></div>
    <div class="box"></div>
  </div>
  <script>
    let boxEles = document.querySelectorAll('.box');
    for (let box of boxEles) {
      box.ondrop = function (ev) {
        console.log('源对象拖动着在目标对象上方释放/松手')
        ev.preventDefault();
        var data = ev.dataTransfer.getData("DIV");
        ev.target.appendChild(document.getElementById(data));
      }
      box.ondragenter = function (ev) {
        console.log('目标对象被源对象拖动着进入')
      }
      box.ondragleave = function (ev) {
        console.log('源对象拖动着离开了目标对象')
      }
      box.ondragover = function (ev) {
        console.log('目标对象被源对象拖动着悬停在上方')
        ev.preventDefault();
      }
    }

    target.ondragstart = function(e) {
      console.log('事件源p3开始拖动');
      e.dataTransfer.setData("DIV", e.target.id);
    }
    target.ondrag = function(e) {
      console.log('事件源p3拖动中');
    }
    target.ondragend = function() {
      console.log('源对象p3拖动结束');
    }
  </script>
</body>
</html>
```



## 在页面中拖动元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    .container {
      position: relative;
    }
    .target {
      position: absolute;
      top: 0;
      left: 0;
      width: 100px;
      height: 100px;
      background: #000;
    }
  </style>
</head>
<body class="container">
  <div class="target" id="target" draggable="true"></div>
  <script>
    target.ondragstart = function (e) {
      // 记录拖动前的坐标
      offsetX = e.offsetX;
      offsetY = e.offsetY;
    }
    target.ondrag = function (e) {
      // 获取拖动源在页面中的位置
      var x = e.pageX;
      var y = e.pageY;
      console.log(x + '-' + y);
      if (x == 0 && y == 0) {
        return; // 不处理拖动最后一刻X和Y都为0的情形
      }
      x -= offsetX;
      y -= offsetY;
      // 更新拖动源的位置
      target.style.left = x + 'px';
      target.style.top = y + 'px';
    }
    target.ondragend = function () {
      console.log('源对象p3拖动结束');
    }
  </script>
</body>
</html>
```





# 参考资料

[HTML5-draggable(拖放)](http://www.cnblogs.com/blog-leo/p/4457697.html) 

[HTML5 drag & drop 拖拽与拖放简介](http://www.zhangxinxu.com/wordpress/2011/02/html5-drag-drop-%E6%8B%96%E6%8B%BD%E4%B8%8E%E6%8B%96%E6%94%BE%E7%AE%80%E4%BB%8B/) 

[HTML5--拖拽API(含超经典例子)](http://blog.csdn.net/baidu_25343343/article/details/53215193) 