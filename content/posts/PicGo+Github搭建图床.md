---
title: picgo+github搭建图床
index_img: 'https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/2203221710.jpg'
banner_img: 'https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/2203221710.jpg'
tags:
  - 教程
description: 利用github搭建免费个人图床
slug: 584214c1
date: 2021-03-18
---
1. 登录github账号，新建一个放图片的仓库

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181819.png)

2. 一般创建成功之后，会出现如下界面

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181820.png)

3. 下载并安装[PicGo](https://molunerfinn.com/PicGo/)

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181821.png)

4. 去github获取token

   打开 `Settings -> Developer settings -> Personal access tokens`，最后点击 `generate new token`

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181825.png)

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181827.png)

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181828.png)

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181830.png)

   `token` 生成，注意它只会显示一次，所以你最好把它复制下来到你的备忘录存好，方便下次使用，否则下次有需要重新新建；

5. 配置PicGo

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181831.png)

   ![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803181832.png)

6. 加速访问

   直接访问github仓库的图片太慢了，所以我们用[jsDelivr](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.jsdelivr.com%2F)进行加速访问，直接在PicGo设置的自定义域名中设置

   <code>https://cdn.jsdelivr.net/gh/用户名/仓库名</code>

   上传之前最好对图片进行压缩，个人推荐[TinyPNG](https://tinyjpg.com/)。

