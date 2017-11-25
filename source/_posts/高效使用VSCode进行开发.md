---
title: 高效使用VSCode进行开发
categories:
  - 开发工具
tags:
  - VSCode
  - 开发工具
---
# [VSCode](https://code.visualstudio.com/Download)简介

`VSCode`是微软推出的一款产品，他不是一款强大的`IDE`，而是支持按需扩展各种插件的轻量级编辑器，可以看作是`Visual Studio`的精简版，界面风格相同。竞争对手会是`Sublime`、`Atom`，提供丰富的插件和优雅的编辑界面，内置多项丰富的功能，包括版本控制、插件管理、代码调试、多项目管理(v1.8)等。

## 为什么选择VSCode

之前一直很排斥微软的产品，特别是`IE`和`.Net`。不过`VSCode`的确是微软的一款良心作品，轻巧、简洁、高效。以前用过很多`编辑器`和`IDE`，从最开始的`Notepad++`，到后面功能强大得无敌的`eclipse`、`Zend Studio`、`HBuilder`等E系的产品，不过感觉速度特变慢，又大又臃肿，后面接触了`IntelliJ `的相关产品(`webStrom`、`phpStrom`、`Android Studio`等等)，感觉速度快多了，也很强大，但是却不支持工作区多开，这点特别不爽。一圈转下来，感觉最好的莫过于`Sublime`。直到最近开始了解`VSCode`，开始慢慢喜欢上了这款编辑器，也对微软的产品有了一个全新的认识。



# 使用指南

## 用户代码片段

文件 - 首选项 - 用户代码片段 - 选择相关语言

编辑此`json`文件即可，例:

```json
{
  "Print to console": {
    "prefix": "log",
    "body": [
      "console.log($1)$2"
    ],
    "description": "Log output to console"
  }
}
```

之后只要在`js`代码中使用`log`，即可输出`console.log()`，光标会在`$1`的位置停留，再按下`tab`键跳转到结尾部分。



## VSCode快捷键

### 窗口操作

```json
Ctrl + Shift + N # 新建窗口
Ctrl + Shift + W # 关闭窗口
```

### 编辑器控制

```json
Ctrl + N # 新建文件
Ctrl + Tab # 打开的文件间切换
Ctrl + \ # 新建编辑器
Ctrl + 1，Ctrl + 2，Ctrl + 3 # 编辑器切换
Ctrl + K + Right # 编辑器切换
```

### 格式调整

```json
Ctrl + [，ctrl + ] # 代码缩进
Ctrl + Shift + [，Ctrl + Shift + ] # 折叠打开代码块
Alt + Shift + F # 代码格式化
Alt + Up，Alt + Down # 上移一行，下移一行
Ctrl + Enter # 在当前行后面插入空行
Ctrl + Shift + Enter # 在当前行前面插入空行
```

### 编辑

```json
Ctrl + C，Ctrl + V，Ctrl + X # 如果不选中，默认复制或剪切一整行
Ctrl + Delete，Ctrl + Backspace # 删除一个单词
Ctrl + Shift + K # 删除当前选中行
Ctrl + Shift + D # 复制当前行并插入到下一行
Ctrl + / # 单行注释
Ctrl + Shift + / # 多行注释
```

### 光标

```json
Ctrl + 鼠标左键 # 多点编辑
Ctrl + I # 选择当前行
Ctrl + D # 选中下一个匹配的单词
Ctrl + U # 回退上一个光标操作
Alt + Shift + Up，Alt + Shift + Down # 光标向上或向下多选一行
Ctrl + Home，Ctrl + End # 光标转到文件头部或尾部
Home，End # 光标转到行首或行尾
```

### 查找/替换

```json
Ctrl + Shift + F # 全局搜索
Ctrl + H # 替换
Ctrl + F # 查找
Ctrl + G # 转到指定行
```

### 重构

```json
F12 # 转到定义
Shift + F12 # 列出所有引用
F2 # 重命名
```

### 显示

```json
F11 # 全屏显示
Ctrl + =/+/- # 缩放
Ctrl + B # 打开、隐藏侧边栏
Ctrl + ` # 打开、隐藏终端
```



## VSCode命令窗口

打开命令窗口

```bash
Ctrl + Shift + P
```

### 基础

```json
format # 代码格式化
```

### emmet

```json
Emmet: Expand Abbreviation # 代码展开
```



# 参考资料

- 博客园 - 玄奘Plus : [VSCode 插件](http://www.cnblogs.com/it-jason/p/6722347.html)

- 知乎专栏 : [VS Code 使用小技巧](https://zhuanlan.zhihu.com/p/22880087)

- 官方文档（英文版）：[Documentation for Visual Studio Code](http://link.zhihu.com/?target=https%3A//code.visualstudio.com/docs)

- 中文文档（未完成）：[GitHub - jeasonstudio/CN-VScode-Docs: VScode说明文档翻译](http://link.zhihu.com/?target=https%3A//github.com/jeasonstudio/CN-VScode-Docs)