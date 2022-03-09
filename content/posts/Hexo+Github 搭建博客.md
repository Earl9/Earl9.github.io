---
title: Hexo+Github 搭建博客
slug: 1797fdb5
index_img: https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/2203221712.jpg
banner_img: https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/2203221712.jpg
tags:
  - 教程
categories:
  - Hexo,Git,NPM
description: "“0”基础搭建个人博客"
date: 2020-07-16
---
## 前言

使用github pages服务搭建博客的好处：

1. 全是静态文件，访问速度快了；
2. 免费，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器；
3. 数据绝对安全，基于github的版本管理；
4. 博客内容可以轻松打包、转移、发布到其它平台；

## 准备环境

首先安装[Node.js®](https://nodejs.org/en/download/)

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182016.png)

安装过程直接点next就行，有需要的可以改一下安装位置。

安装[Git](https://git-scm.com/)

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182017.png)

安装过程同上。

在安装完 `Git `和 `Node.js` 后在任意文件夹右键打开 Git bash here 即可打开 Git 的 Bash 终端

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182018.png)

然后在git终端输入以下命令，查看查看 git 和 npm 版本号：

```
git --version
NPM -v
```

如果你的配置没问题，就会得到如下运行结果：

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182019.png)



## 安装hexo

完成上面操作后，就可以安装hexo了，也可查看[hexo](https://hexo.io/zh-cn/)官方文档。

在Git Bash 里执行以下命令：

```
npm install -g hexo-cli
```

hexo安装完成后，在想要存储 hexo 文件的目录打开 Git Bash，然后初始化hexo，比如我想要在 D 盘的一个名为 hexo 的文件夹进行写作，那我在 D 盘打开 Git Bash 使用下面的命令初始化 Hexo 博客：

```
hexo init hexo
```

新建完成后，指定的文件夹目录如下：

```
.
├── _config.yml # Hexo的全局配置文件，在此配置网站的主要参数
├── package.json # npm软件包以及版本信息
├── scaffolds # 模版文件夹
├── source  # 资源文件夹
|   ├── _drafts # 草稿/模板文件
|   └── _posts # 文章Markdowm文件 
└── themes  # 主题文件夹

```

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182020.png)

然后安装一些组件

```
npm install
```

安装好后，执行以下命令

```
hexo g #generate 生成静态文件
hexo s #server 启动服务器。
```

默认情况下，访问网址为： [http://localhost:4000/](http://localhost:4000/)，打开网址，页码如下图：

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182022.png)

如果上面 `hexo s `没有成功，可能是本地的 4000 端口被占用，请使用 `hexo s -p 端口号` 修改本地预览的端口号，然后打开浏览器，输入 `http://localhost:你的端口号`来启动本地预览.

至此，你的本地博客已经搭建成功，接下来是部署到Github Page。

## 关联Github

首先注册一个github账号（如何注册这里就不说了）

登录到github，新建一个名为` username.github.io`的公开仓库（私有仓库不能访问）其中`username` 是你的 github 用户名，例如我的用户名是 `Test`，那么我新建的仓库名称即为`Test.github.io`

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182023.png)

链接github与本地

打开Git Bash，输入以下命令：

```
git config --global user.name "username" # username是你的Github用户名，注意大小写保持一致
git config --global user.email "your email address" # your email address填写你的Github注册用的邮箱
ssh-keygen -t rsa -C "your email address"  # 生成SSH公钥，your email address同上填
```

完成上面步骤后默认生成的密钥在 `C:\Users\用户名\.ssh\` 目录下，以文本编辑器打开 `id_rsa.pub` 文件，复制里面的所有内容，后面会用到

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182025.png)

在github页面点击头像打开`setting`

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182029.png)

选择 `SSH and GPG Keys`，点击 `New SSH Key`

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182031.png)

`Title`随便填，`Key`填你在本地` id_rsa.pub `文件中复制的所有内容

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182032.png)

打开 Git Bash，输入以下命令:

```
ssh -T git@github.com
```

出现图中的提示代表成功了：(Hi 用户名！You’ve successfully authenticated，but……)

![](https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/1803182033.png)

打开 hexo 目录下的`_config.yml `文件，修改最后一行的 deploy 配置信息：

```
deploy:
  type: git
  repo:
    github: https://github.com/username/username.github.io.git # 将username修改成你的Github用户名
  branch: master 
```


打开 Git Bash，输入如下命令：

```
npm install hexo-deployer-git --save
```

推送到github仓库

```
hexo clean && hexo g && hexo d
```

如果前面没有出错的话，再次执行上面命令，你发现 hexo d 后本地预览的博客已经推送到你的远程仓库了，并且浏览器地址输入`username.github.io `可以访问你的博客了，代表关联 Hexo 到 Github 成功了！

hexo的命令极简单，安装后只需要记住四个常用的基础命令即可。执行命令需要git当前处于blog文件夹根目下

```
hexo g #generate 生成静态文件
hexo s #server 启动服务器。在本地预览效果，默认情况下，访问网址为： http://localhost:4000/
hexo d #deploy 部署网站同步到github。部署网站前，需要预先生成静态文件
hexo clean #clean 清除缓存文件 (db.json) 和已生成的静态文件 (public)。
```

## hexo高级配置

hexo有许多主题，目前大多数人使用`next`主题，本站使用的`butterfly`主题，安装过程如下：

```
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

安装完还需要修改hexo的配置文件`_config.yml`:

```
theme: landscape # 默认的主题
theme:  修改为你的主题名
```

个人推荐主题

* [butterfly](https://github.com/jerryc127/hexo-theme-butterfly) 详细设置可以看[butterfly](https://demo.jerryc.me/)官方文档
* [fluid](https://github.com/fluid-dev/hexo-theme-fluid)  详细设置可以看官方文档[Fluid](https://hexo.fluid-dev.com/docs/start/#%E4%B8%BB%E9%A2%98%E7%AE%80%E4%BB%8B)

插件推荐

* 压缩生成文件[hexo-all-minifier](https://github.com/chenzhutian/hexo-all-minifier)
* 生成文章永久链接[hexo-abbrlink](https://github.com/rozbo/hexo-abbrlink)
* 看板娘[hexo-helper-live2d](https://github.com/EYHN/hexo-helper-live2d)