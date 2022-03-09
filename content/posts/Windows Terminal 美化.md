---
title: "WindowsTerminal美化"
date: 2021-10-19T22:41:18+08:00
draft: false
slug: 353f4593
tags:
  - 教程
categories: Terminal
description: "终端美化，告别传统黑窗口"
---

### 美化效果

![](https://gitee.com/earl9/img/raw/master/img/2021/10195711-6.webp)

### 前提条件

- **Windows Terminal**美化的基础就是**Powerline**。**Powerline**提供自定义的命令提示符体验，提供 Git 状态颜色编码和提示符。成功安装需要具备三个条件：

  >安装Powerline字形——Meslo LGM NF
  >安装 Posh-Git-————将 Git 状态信息添加到提示，并为 Git 命令、参数、远程和分支名称添加 tab 自动补全
  >安装Oh-My-Posh———为 PowerShell 提示符提供主题功能

#### 安装字体

- 在[Nerd Fonts](https://www.nerdfonts.com/font-downloads)选择喜欢的字体下载到** Windows** ，并进行安装。

- 进入**PowerShell**设置选择刚刚安装的字体,然后保存

  ![](https://gitee.com/earl9/img/raw/master/img/2021/10195711-7.webp)

### 安装 posh-git 以及 oh-my-posh

- 如果尚未安装**Git for Windows**，请安装适用于 Windows 的[Git](https://git-scm.com/downloads)。

- 以管理员权限运行**PowerShell**，安装 Posh-Git 和 Oh-My-Posh

  ~~~shell
  Install-Module posh-git -Scope CurrentUser
  Install-Module oh-my-posh -Scope CurrentUser
  ~~~

  >对于安装过程中出现的提示信息，全部选择 [Y]“是”

### 在PowerShell中设置Powerline

- 在**PowerShell**中输入`notepad $profile`,用记事本打开**PowerShell**配置文件，如果你没设置过 **PowerShell**，那么这条命令会为你创建一个新的配置文件 **Microsoft.PowerShell_profile.ps1** ，这个文件是一个脚本，该脚本在每次启动 **PowerShell**时运行。

- 在**PowerShell**配置文件中加入下面内容

  ~~~shell
  Set-PoshPrompt -Theme agnoster
  Import-Module posh-git
  Import-Module oh-my-posh
  ~~~

### 添加配色方案

- 打开[iTerm2-Color-Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes)，找到`windowsterminal`

  ![](https://gitee.com/earl9/img/raw/master/img/2021/10195711-9.webp)

- 打开文件中的Json文件，复制全部内容

  ![](https://gitee.com/earl9/img/raw/master/img/2021/10195711.webp)

- 点击**PowerShell**设置中的按钮编辑配置文件

  ![](https://gitee.com/earl9/img/raw/master/img/2021/10195711-1.webp)

- 找到**"schemes"**这一行

  ![](https://gitee.com/earl9/img/raw/master/img/2021/10195711-2.webp)

- 在`[]`中的最后一行上面的`{}`后面加上英文`,`然后粘贴上面复制的内容，最后进行保存

  ![](https://gitee.com/earl9/img/raw/master/img/2021/10195711-4.webp)

- 最后在**PowerShell**主题设置中找到刚刚添加的配色并进行应用保存

  ![](https://gitee.com/earl9/img/raw/master/img/2021/10195711-5.webp)
