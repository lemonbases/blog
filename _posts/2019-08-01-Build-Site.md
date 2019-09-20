---
layout: post
title: 'jekyll, gitpage搭建博客'
date: 2019-08-01
author: feifei
tags: 网站运维
---

> 使用jekyll, gitpage搭建博客

## Gitpage

![https://hw311.me/assets/imgs/Octocat.png](https://hw311.me/assets/imgs/Octocat.png)
github Pages可以被认为是用户编写的、托管在github上的静态网页。github提供模板，允许站内生成网页，但也允许用户自己编写网页，然后上传。

## Jekyll

![https://hw311.me/assets/imgs/jekyll-logo-light-transparent.png](https://hw311.me/assets/imgs/jekyll-logo-light-transparent.png)
[Jekyll](https://jekyllcn.com/)是一个静态站点生成器，它会根据网页源码生成静态文件。它提供了模板、变量、插件等功能。

## 快速安装

**1 安装ruby开发环境**

```C-like
// for win
// download Ruby+Devkit exe https://rubyinstaller.org/downloads/

// for ubuntu
sudo apt-get install ruby-full build-essential zlib1g-dev

// for mac
// 参考https://www.jekyll.com.cn/docs/installation/macos/
```
**2 安装 Jekyll及jekyll-paginate**

```C-like
gem install jekyll
gem install jekyll-paginate
```
**3 将目标主题`clone`到本地**

```C-like
git clone https://github.com/kaeyleo/jekyll-theme-H2O.git
```
**4 进入目录，开启服务**

```C-like
jekyll server
```
**5 通过`127.0.0.1:4000`预览主题**

## 博文写作

**目录树**

```C-like
	.
	├── _config.yml # 配置文件
	├── _includes # 页面组件方便重用
	|   ├── footer.html # 页脚
	|   └── head.html # html文档的头部内容
	|   └── header.html # 顶部菜单栏
	|   └── pageNav.html # 文章列表分页组件
	├── _layouts # 布局模板
	|   ├── default.html # 默认模板
	|   └── post.html # 文章页面模板
	├── _posts # 这里放文章
	|   └── 2007-02-21-life-on-mars.md # 命名格式：年-月-日-文章标题.md
	├── _site # Jekyll将源码处理后生成的站点文件，里面的内容可直接发布
	├── assets # 存放用于线上环境的静态资源，如需修改css和js文件请到dev文件夹
	|   ├── css # dev文件夹中sass编译后的样式文件
	|   └── fonts # 字体文件
	|   └── icons # 图标文件
	|   └── img #  图片文件
	|   └── js # dev文件夹中处理后的脚本文件
	├── dev # 开发文件
	|   ├── js # 存放脚本源码
	|   └── sass # 样式源码
	|       └── app.scss # 整合下面的所有样式文件
	|       └── base.scss # 引入字体、Reset部分样式
	|       └── common.scss # 模板的主要样式
	|       └── helper.scss # 工具样式
	|       └── layouts.scss # 响应式布局
	└── gulpfile.js # 自动化任务脚本
	└── index.html # 模板首页
	└── tags.html # 标签页面
	└── 404.html # 404页面
	└── package.json # 管理项目的依赖项
```

博文的markdown文件存储于`_post`文件夹，每篇博文需设置头信息：
```C-like
layout: default
home-title: lemonbases blog
description: Hello World
header-img: assets/img/banner.jpg
```

写作完成后，使用
```C-like
jekyll s 
// or
jekyll buld
```
生成目标网页

## 参考资料

- [https://github.com/kaeyleo/jekyll-theme-H2O](https://github.com/kaeyleo/jekyll-theme-H2O)
- [使用Jekyll和GitHub Pages搭建博客](https://hw311.me/zh/jekyll/2019/01/21/blog-jekyll-github-pages/)
- [Jekyll quickstart](https://jekyllrb.com/docs/)