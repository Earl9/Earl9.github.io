---
title: ArchLinux+Windows10双系统安装笔记
slug: 307d4fd8
index_img: 'https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/2203221707.jpg'
banner_img: 'https://cdn.jsdelivr.net/gh/Earl9/img/2021/03/2203221707.jpg'
tags:
  - 教程
categories: Linux
description: "archLinux探索"
date: 2021-02-13
ShowToc: true
TocOpen: true
---

1. 在磁盘管理分区，大小100G(随意大小)左右
2. 去[官网](https://archlinux.org/)下载镜像，使用balenaEtcher将镜像写入U盘
3. 调整win10的EFI分区大小为500MB（谨慎操作，建议删除分区后重建，然后用dism++恢复引导）
4. bios进行相关设置，开机选择从U盘启动
5. 进入安装页面，开始输入命令安装

* 连接WiFi

  ~~~ bash
  itwctl
  device list
  station <devicename#一般是wlan0> scan
  station <devicename> get-networks
  station <devicename> connect <wifi名称> #如果知道devicename和WiFi名字,可以直接执行这一步
  exit
  ~~~

* 设置国内源

  ~~~ bash
  reflector -c China -a 6 --sort rate --save /etc/pacman.d/mirrorlist #使用reflector来获取速度最快的6个镜像并保存到mirrorlist文件中。或：
  nano /etc/pacman.d/mirrorlist #进入文件将第一个网址中间部分改为163,Ctrl+X保存#也可使用vim命令修改首先安装vim: pacman -S vim
  ~~~

* 磁盘准备

  ~~~bash
  lsblk #机械硬盘一般为sda，固态硬盘为nvme
  cfdisk /dev/nvme0n1
  找到之前划分的空磁盘，选择new回车，输入大小回车，然后选择write回车，输入yes回车，最后选择quit回车退出
  lsblk
  mkfs.ext4 /dev/nvme0n1p5 #将刚刚分好的区格式化为ext4格式
  ~~~

* 挂载分区

  ~~~bash
  mount /dev/nvme0n1p5 /mnt #上面新建的分区
  lsblk #找到win10的EFI分区
  mkdir /mnt/boot
  mount /dev/nvme0n1p2 /mnt/boot #挂载Win10的EFI分区
  ~~~

* 安装系统

  ~~~bash
  pacstrap /mnt base linux linux-firmware nano #linux-firmware为默认内核，推荐使用linux-lts
  ~~~

* 生成fstab文件

  ~~~bash
  genfstab -U /mnt >> /mnt/etc/fstab
  cat /mnt/etc/fstab #检查生成的fstab是否正确（每个分区占一行）
  ~~~

* 配置新系统

  ~~~bash
  arch-chroot /mnt
  timedatectl set-timezone Asia/Shanghai #设置时区
  hwclock --systohc #同步硬件时钟
  nano /etc/locale.gen
  Ctrl+W 输入 #en_US 回车 找到UTF-8那一行 删掉前面的#（取消注释）
  Ctrl+W 输入 #zh_CN 回车 找到UTF-8那一行 删掉前面的#（取消注释）
  Ctrl+X保存退出
  locale-gen
  nano /etc/locale.conf
  填入内容：LANG=en_US.UTF-8
  nano /etc/hostname #创建并写入hostname，(arch)
  nano /etc/hosts #修改hosts(中间不是空格，而是tab)
  127.0.0.1	localhost
  ::1		localhost
  127.0.1.1	arch.localdomain	arch
  Ctrl+X保存退出
  passwd #为root用户创建密码
  pacman -S grub efibootmgr networkmanager network-manager-applet dialog wireless_tools wpa_supplicant os-prober mtools dosfstools ntfs-3g base-devel linux-headers reflector git sudo #安装基本的包，使用grub引导，命令比较长，不要输错
  grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Arch
  grub-mkconfig -o /boot/grub/grub.cfg
  exit
  umount -R /mnt
  reboot #拔出U盘
  ~~~

* 配置安装好的系统

  ~~~bash
  systemctl start NetworkManager #启动服务
  systemctl enable --now NetworkManager #开机自启动服务
  nmcli device wifi list #连接WiFi
  nmcli device wifi connect wifi名 password wifi密码
  useradd -m -G wheel -s /bin/bash username #创建普通用户
  passwd username
  EDITOR=nano visudo
  Ctrl+W输入 # %wheel 回车找到这行,删除前面的#（取消注释）
  Ctrl+X保存退出
  ~~~

* 安装显卡驱动以及桌面

  ~~~bash
  pacman -S xf86-video-intel(amdgpu) #集显驱动
  pacman -S nvidia nvidia-utils #NVIDIA独显驱动
  pacman -S xorg #安装显示服务器
  pacman -S sddm #安装kde桌面环境
  systemctl enable sddm #开机启动
  pacman -S plasma kde-applications packagekit-qt5 #桌面环境
  pacman -S ttf-sarasa-gothic #安装字体：更纱黑体
  reboot
  ~~~

5. 开机即可进入桌面环境了

   

