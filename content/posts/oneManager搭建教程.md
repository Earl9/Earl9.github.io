---
title: "OneManager搭建教程"
date: 2021-10-19T22:44:25+08:00
draft: false
slug: 0adbf5e5
tags:
  - 教程
categories: Linux
description: "利用OneManager搭建网盘"
---

> ​		`OneManager`是一种能够挂载多种网盘的程序，可以通过网站直接去访问`onedrive`的文件，从而实现直链下载，网页在线观看视频等。
>
> ​		前提条件：`onedrive`账号，VPS一台。
>
> ​		项目地址[oneManager](https://github.com/qkqpttgf/OneManager-php),` oneManager`的部署方式有很多种，本文主要介绍在VPS上利用[宝塔面板](https://www.bt.cn/)来部署的方法。

- 首先进入宝塔首页，安装好基本运行环境`Nginx`和`PHP`,然后进入网站页面

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-18.webp)

- 点击添加站点，填好域名，`PHP`版本随意

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-19.webp)

- 点击根目录进入网站文件地址

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-14.webp)

- 进入[oneManager](https://github.com/qkqpttgf/OneManager-php)项目地址将项目压缩包下载到本地

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-9.webp)

- 然后进入宝塔将压缩包上传到网站根目录

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-13.webp)

- 上传完成后将压缩包解压，得到网站文件

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-5.webp)

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-11.webp)

- 然后打开网站设置修改网站目录

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-17.webp)

- 并进行伪静态设置

  `Nginx`伪静态规则

  ~~~java
  rewrite ^/(.*) /index.php?/$1 last;
  ~~~

  `Apache`伪静态规则

  ~~~java
  RewriteEngine On
  RewriteRule ^(.*) index.php?/$1 [L]
  ~~~

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-10.webp)

- 网站设置到这来就结束了，接下来访问网站域名进行`oneManager`程序安装与网盘挂载

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-8.webp)

- 选择简体中文

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-12.webp)

- 确认已启用伪静态


  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-16.webp)

- 然后设置网站管理员密码

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-20.webp)

- 接着进入首页点击登录进入后台管理页面

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-7.webp)

- 选择`onedrive`然后点击添加盘，输入基本信息

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-15.webp)

- 接着登录`onedrive`账号

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-21.webp)

- 获取权限

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-2.webp)

  

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-22.webp)

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016.webp)

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-1.webp)

- 到这里就安装完成了，然后进入后台可以进行一些个性化设置

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-3.webp)

  ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/10/20210016-4.webp)

