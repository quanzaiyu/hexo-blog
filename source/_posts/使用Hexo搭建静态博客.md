---
title: 使用Hexo搭建静态博客
categories:
  - Hexo
tags:
  - Hexo
---
> 这不是一个教程，只是记录个人配置Hexo的历程，方便下次使用，最后的参考资料中会有详细的搭建教程。

# Hexo简介

hexo是一款快速、简洁且高效的博客框架，基于node.js构建，速度快，支持Markdown的编写文章，可以一键部署到GitHub，简单方便。参考 : [Hexo中文网站](https://hexo.io/zh-cn/)



# 环境搭建

依赖环境: `node.js`、`npm`(或`yarn`)，部署网站需要 `git`



## 安装和使用

### 安装

```bash
yarn add global hexo-cli # 安装hexo-cli
hexo -v # 检测hexo是否安装完成
```



### 初始化站点

```bash
hexo init <folder> # 新建一个Hexo站点，如果没有设置 <folder>，Hexo 默认在目前的文件夹建立网站。
cd <folder> && npm install

hexo server # 启动服务器，默认情况下，访问网址为： http://localhost:4000/
# 可选参数:
# -p, --port	重设端口
# -s, --static	只使用静态文件
# -l, --log		启动日记记录，使用覆盖记录格式
```



### 其他命令

```bash
hexo new [layout] <title> # 新建一篇文章
# 如果没有设置 [layout] 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话需要使用引号括起来。

hexo generate # 生成静态文件
# 可选参数:
# -d, --deploy	文件生成后立即部署网站
# -w, --watch监视文件变动

hexo publish [layout] <filename> # 发布草稿

hexo deploy # 部署网站
# 可选参数:
# -g, --generate	部署之前预先生成静态文件

hexo clean # 清除缓存，这将清除缓存文件 (db.json) 和已生成的静态文件 (public)。
```



### 命令简写

```bash
hexo n # hexo new
hexo g # hexo generate
hexo s # hexo server
hexo d # hexo deploy
```



## 网站部署配置

编辑项目根目录下的`_config,yml`

```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/quanzaiyu/quanzaiyu.github.io.git
  barnch: master
```

可能需要配置`SSH`，网上搜一下就有解决方案。[参考](https://help.github.com/articles/connecting-to-github-with-ssh/)

关于如何配置 `github pages` 参考网上资料即可。

可能出现的问题:

> (node:19344) [DEP0061] DeprecationWarning: fs.SyncWriteStream is deprecated

根据提示，`deprecated`，弃用了，说明前面那个应该是个方法，这个方法在node当中弃用了。明显就是Hexo还在继续使用这个已经被弃用的方法。

解决方案: `npm install hexo-fs --save` 更新一下`hexo-fs`插件。[参考](http://www.abrocks.com/2017/06/17/node8.0%E6%96%B9%E6%B3%95%E5%BC%83%E7%94%A8%E5%A4%84%E7%90%86/) 

> **Error**: Deployer not found : github,

安装插件 `npm install hexo-deployer-git --save` ，然后再`generate` 和`deploy`



# 参考资料

- 博客园 - [cherishzy](http://www.cnblogs.com/cherishzy/) : [在Github上面搭建Hexo博客（一）：部署到Github](http://www.cnblogs.com/cherishzy/p/5694658.html)
- 博客园 - [流浪猫の窝](http://www.cnblogs.com/liulangmao/) : [使用hexo搭建github.io博客(一)](http://www.cnblogs.com/liulangmao/p/4323064.html)
- 简书 - [sanchuan](http://www.jianshu.com/u/60e19f1dfd2a) : [你不知道的HEXO deploy](http://www.jianshu.com/p/da941bd0a3dd)