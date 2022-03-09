---
title: "Git笔记"
date: 2021-12-05T22:48:22+08:00
draft: true
slug: e7ee00fb
tags:
  - Git
categories: Git
---

## 1. 解决什么问题

- Git解决了团队开发过程中代码管理的问题

## 2. Git是什么

-  Git是开发中进行代码管理的工具

## 3. Git怎么用

### 3.1 Git 下载安装

- 下载地址[Git](https://git-scm.com/)

- 注意：

  >1. 安装目录不要有中文
  >
  >2. 不要做任何修改，直接下一步

- 验证：

  ~~~git
  git --version
  ~~~

- 安装和配置Git图形化的工具时注意，设置用户名和邮箱

  >name:	example
  >
  >email:	example@example.com[能够收到邮件]
  >
  >（后期可以通过命令行的方式修改）

  ~~~shell
  git config --global user.name "exampleUserName"
  git config --global user.email example@example.com
  ~~~

### 3.2 Git原理和基础概念

- 本地仓库：local repository 		.git目录就是本地仓库

- 工作目录：work tree				  .git所在目录

- 暂存区：index						    .git目录中的index文件

  >- Git官网给我们提供一本书：[Git Pro](https://git-scm.com/book/zh/v2) 中文版书籍提供了Git基本使用方法。【不用记命令】
  >
  >- [Git命令查找](https://gitexplorer.com/)

#### 使用基本流程

<img src="https://gitee.com/earl9/img/raw/master/img/2021/11212419.png" alt="image-20211101151135715" style="zoom:80%;" />

- 建议掌握命令

### 3.3 分支

- 创建分支

  ~~~shell
  git branch 分支名称
  ~~~

- 查看分支

  ~~~shell
  git branch
  ~~~

- 删除分支

  ~~~shell
  git branch -d 分支名称
  ~~~

- 切换分支

  ~~~shell
  git checkout 分支名称
  git switch 分支名称
  ~~~

- 合并分支

  ~~~shell
  git merge  分支名称
  ~~~

### 3.4 常用命令

- 获取提示

  ~~~shell
  git --help	
  git 命令 --help
  ~~~

- 初始化仓库

  ~~~shell
  git init
  ~~~

- 查看工作目录状态

  ~~~shell
  git status
  ~~~

- 把工作目录文件添加到暂存区

  ~~~Shell
  git add 文件名
  git add .	//添加所有文件
  ~~~

- 提交到本地仓库

  ~~~Shell
  git commint -m '提交信息' 
  ~~~

- 展示提交信息日志

  ~~~shell
  git log
  ~~~

- 移除暂存区文件

  ~~~shell
  git rm --cached 文件名
  ~~~

- 回到过去版本

  ~~~shell
  git reflog
  git reset --hard 文件唯一索引
  ~~~

