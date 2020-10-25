---
layout: post
title: '基于Jekyll的GitHub博客搭建'
date: 2020-10-25
author: 廉兆
categories: 技术
cover: '/assets/img/2020-10-25-GitHub-Pages/library.jpg'
tags: Jekyll GitHub 博客
---

> 基于Jekyll的Github博客搭建全过程及踩过的坑

## 基本步骤

1. 本地安装Jekyll
2. 下载主题模板并进行配置
3. 实现本地预览
4. 写文章
5. 发布到GitHub

## 1. 本地安装Jekyll

> Jekyll 是一个静态网站生成器。用你喜欢的 标记语言书写内容并交给 Jekyll 处理，它将利用模板为你创建一个静态网站。你可以 调整你想要的网址样式、在网站上显示哪些数据 等等。

具体安装步骤可以参考官网[快速入门](https://www.jekyll.com.cn/docs/)

**注意：**
由于Ruby的包管理工具安装Jekyll软件包速度比较慢，可以尝试切换到国内的软件源进行安装

## 2. 下载主题模板并进行配置

[Jekyll Theme](http://jekyllthemes.org/)  
选择一个喜欢的主题  
[Theme H2O](https://github.com/kaeyleo/jekyll-theme-H2O)  
我选用的是这个模板，很漂亮，详细的介绍见链接

直接下载文件到本地，按照文档进行个性化配置即可

## 3. 实现本地预览

如果你已经安装好了Jekyll，那么可以直接在博客所在目录运行指令`jekyll serve`

这样就会在本地的`127.0.0.1:4000`运行一个服务器，可以直接在浏览器中进行本地访问

## 4. 写文章

所有要发表的文章都存放在`_posts`文件夹中

文件名使用`yyyy-mm-dd-文章名.md`的格式

每篇文章的开头都需要设置一些头信息：

```
---
layout: post
title: '基于Jekyll的GitHub博客搭建'
date: 2020-10-25
author: 廉兆
categories: 技术
cover: '/assets/img/2020-10-25-GitHub-Pages/library.jpg'
tags: Jekyll GitHub 博客
---
```

其中的信息包括：
```
layout: 渲染使用的布局
title: 文章名
date: 发表日期
author: 作者
categories: 分类
cover: 封面
tags: 标签
```

## 5. 发布到Github

1. 在GitHub建立形如`用户名.github.io`的仓库
2. 提交本地文件到刚建立的仓库
3. 打开与建立的仓库同名的网址进行即可看到最终的发布效果