---
title: Weex开发实践01 - 环境搭建
categories:
  - Weex
tags:
  - Weex
---



# Weex简介

Weex 是一套简单易用的跨平台开发方案，能以 web 的开发体验构建高性能、可扩展的 native 应用，为了做到这些，Weex 与 Vue 合作，使用 Vue 作为上层框架，并遵循 W3C 标准实现了统一的 JSEngine 和 DOM API，打造三端一致的 `native` 应用。



# 环境搭建

## 安装weex-toolkit

```bash
npm install -g weex-toolkit
weex -v // 查看当前weex版本
weex // 获取weex帮助
```



## weex-toolkit的使用

### 升级子依赖模块

```bash
weex update weex-devtool@latest // @后标注版本后，latest表示最新
```



### 初始化weex项目

```bash
weex create weex-proj-test
cd weex-proj-test
npm install
```



### 添加平台支持

```bash
weex platform add ios
weex platform add android
```



### 关于 [weexpack](https://www.npmjs.com/package/weexpack) 

weexpack 是新一代的weex应用工程和插件工程开发套件，是基于weex快速搭建应用原型的利器。它能够帮助开发者通过命令行创建weex应用工程和插件工程，快速打包 weex 应用并安装到手机运行，对于具有分享精神的开发者而言还能够创建weex插件模版并发布插件到weex应用市场。 使用weexpack 能够方便的在在weex工程和native工程中安装插件。

目前[weex-toolkit](https://github.com/weexteam/weex-toolkit)集成对weexpack的命令调用支持，你可以使用weex-toolkit命令来实现weexpack具备的功能。比如我们要实现添加iOS应用模板：

```bash
# 使用weexpack 命令
weexpack platform add ios
 
# 使用weex-toolkit
weex platform add  ios
```

又或者添加 weex-action-sheet插件

```bash
# 使用weexpack 命令
weexpack plugin add weex-action-sheet
 
# 使用weex-toolkit
weex plugin add  weex-action-sheet
```



### 运行

#### web 平台

```bash
npm run build //web工程打包
npm run dev & npm run serve 
```

未修改的话默认为: http://192.168.1.23:8081/ 

#### 

#### Android 平台

```bash
weex run android
```

注意: 需要安装对应的Android开发包，并配置环境变量`JAVA_HOME`和`ANDROID_HOME `。



#### iOS 平台

```bash
weex run ios
```

注意: 需要安装Xcode以确保拥有ios的开发环境。



### 打包

#### web 平台

```bash
npm run build
```



#### Android 平台

```bash
# 打包+构建
$ weex build android
# 打包+构建+安装执行
$ weex run android
```



#### iOS 平台

```bash
# 打包+构建
$ weex build ios
# 打包+构建+安装执行
$ weex run ios
```






# 参考资料

[Weex官方开发文档](http://weex.apache.org/cn/guide/) 

[WEEX快速创建工程 Hello World](https://segmentfault.com/a/1190000010984857) 

[网易严选App感受WEEX 开发](https://juejin.im/entry/59af918c5188251240635a96) 

[可能是史上最全的weex踩坑攻略](http://www.jianshu.com/p/497f1a9ff33f) 

[GitHub: yanxuan-weex-demo](https://github.com/zwwill/yanxuan-weex-demo) 

[GitHub: Weex-Toolkit](https://github.com/weexteam/weex-toolkit) 

[NPM: weexpack](https://www.npmjs.com/package/weexpack) 

