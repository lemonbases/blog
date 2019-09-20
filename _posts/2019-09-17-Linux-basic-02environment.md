---
layout: post
title: 'Linux基础-02环境搭建'
date: 2019-09-17
author: 柠檬菌
tags: Linux
---

> 工欲善其事，必先利其器。在开始学习Linux前，首先配置Linux环境。

这里我们介绍两种方式：

- 使用VMware创建虚拟机，然后在虚拟机上安装Linux系统，这里以Ubuntu-18.04为例
- 使用Win10的WSL（Windows Subsystem for Linux）

MAC的同学可以直接使用自带的工具`Terminal`来学习基本的Linux命令

<font color="red">本篇教程截图较多，流量预警</font>

### VMware + Ubuntu-18.04

1. 软件准备

- VMware Workstation Player 15 :  <https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html>

- Ubuntu-18.04镜像地址：
  - 清华镜像：<https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/18.04/>
  - 网易镜像：<https://mirrors.163.com/ubuntu-releases/18.04/>

2. 开启CPU虚拟化支持

开机界面F2或F10进行`BIOS`，`Configuration`选项或`Security`选项将Virtualization `Enabled`，保存BIOS设置，重启计算机。

![bios_virtualization.png](https://ae01.alicdn.com/kf/Hc45aed45bc404482b35eca1d1574cf00q.png)

3. 安装Ubuntu-18.04

- 选择创建新的虚拟机

![install-01](https://ae01.alicdn.com/kf/H1523e4f5dc8e42bdae8c693956a9f4049.png)

![install-02](https://ae01.alicdn.com/kf/Hccc07f36f645468186a21678282d4d0aQ.png)

![install-03](https://ae01.alicdn.com/kf/Heba76e36841846e9b2144d18da6fcca1S.png)

![install-04](https://ae01.alicdn.com/kf/Ha7e79d56c5c24d59923d1f95106b8beex.png)

![install-05](https://ae01.alicdn.com/kf/Hea27d5142a4a4a6d98048cfc41a6bfe5r.png)

![install-06](https://ae01.alicdn.com/kf/Hdd60b056cc0c4317ae2d72ba782fa447W.png)

![install-07](https://ae01.alicdn.com/kf/Hf8136c65fd9448d4998ede27fd9acdf0I.png)

- 根据个人电脑配置选择虚拟机的硬件配置

![install-08](https://ae01.alicdn.com/kf/H835c9f55c82948c1acd94f8a0555ce687.png)

![install-09](https://ae01.alicdn.com/kf/H1cb721a518944fef898fffec7a7bed381.png)

![install-10](https://ae01.alicdn.com/kf/H3d547094b86f4da0896af46e321b772fY.png)

![install-11](https://ae01.alicdn.com/kf/Hcfdccffa8e0d458fae0ec42351b66ad2W.png)

![install-12](https://ae01.alicdn.com/kf/Hfa25456ef6a74ff7bfa358a7ec3e29cby.png)

![install-13](https://ae01.alicdn.com/kf/H7cc512a4268f43a8a9e70a406caae9a1M.png)

![install-14](https://ae01.alicdn.com/kf/H88d253a92d90429b9d2443f0abf6ff2aH.png)

![install-15](https://ae01.alicdn.com/kf/H0a027b2e50174da3be1287c9c3c40dacA.png)

![install-16](https://ae01.alicdn.com/kf/Ha29d669c924e498d9f002c1b6114e0985.png)

![install-17](https://ae01.alicdn.com/kf/H903297333fb94e678418a396e505af3d8.png)

![install-18](https://ae01.alicdn.com/kf/Hb5fb449e657a4b74857641c2f110b35cs.png)

3. 安装VmTools

VMware Tools 是用于增强虚拟机的客户机操作系统性能并改进虚拟机管理的实用程序套件。如果不在客户机操作系统中安装 VMware Tools，客户机性能将缺少重要的功能。安装 VMware Tools 会避免出现以下问题或改进性能：

- 视频分辨率低
- 色深不足
- 网速显示错误
- 鼠标移动受限
- 不能复制、粘贴和拖放文件
- 没有声音
- 提供创建客户机操作系统静默快照的能力
- 将客户机操作系统中的时间与主机上的时间保持同步

![vmtools-01](https://ae01.alicdn.com/kf/H5c5ddb32f7974d5ab59f676dd0c3e7c95.png)

![vmtools-02](https://ae01.alicdn.com/kf/H24703a981cc440be9f93811f21fe6123N.png)

![vmtools-03](https://ae01.alicdn.com/kf/H7257c387ed784ce6bf103392e75f34a1Y.png)

![vmtools-04](https://ae01.alicdn.com/kf/Hfe3daba8b6b141d891365d6c6caeb5d9J.png)

![vmtools-05](https://ae01.alicdn.com/kf/Ha48f5b2c3af64a56bc0c2170d9ba5d78p.png)

![vmtools-06](https://ae01.alicdn.com/kf/H6b1bce0d9afb4aefa86b9ab6426d4338j.png)

### Win10中WSL安装

控制面板->程序和功能->启用或关闭Windows功能->勾选 适用于Linux的Windows子系统

重启电脑后，打开应用商城搜索“WSL”，根据自己需求选择安装一个或多个Linux系统，第一次运行需要等待安装并设置用户名、密码。还可以在cmd中通过`wsl`或`bash`运行WSL。

本小节安装较为简单，因此没有给出相应截图，见谅> <

### 关机和重启

首先我们学习一下命令行关机重启命令。

大多数Linux发行版的默认shell都是GNU bash shell，其能提供对Linux系统的交互式访问。启动`Terminal`后，会看到shell CLI提示符`$`：

```bash
#Ubuntu
learn-linux@learnlinux-PC:~$
#CentOS
[learn-linux@learnlinux-PC ~]$
```

提示符中还显示了当前用户ID`learn-linux`，系统名为`learnlinux-PC`

- 关机命令：

```bash
shutdown -h now
halt
poweroff
init 0
```

- 重启命令：

```bash
reboot
shutdown -r now
init 6
```


