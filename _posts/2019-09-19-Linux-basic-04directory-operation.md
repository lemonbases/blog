---
layout: post
title: 'Linux基础-04目录管理'
date: 2019-09-19
author: 柠檬菌
tags: Linux
---

这里需要了解`绝对路径`和`相对路径`两个概念。

<span><font color="red">绝对路径</font>： 从根目录`/`写起，比如`/usr/bin`</span>

<span><font color="red">相对路径</font>： 基于当前位置的路径表示，`.`表示当前目录，`..`表示当前目录的父目录，双点符可以联用向上切换目录，比如`../Downloads`表示上一层目录中的Downloads文件夹</span>

### ls 显示目录内容

```bash
ls [-a] [目录名]
-a : 全部文件，连同隐藏文件(开头为.的文件)一起列出来
-d : 仅列出目录本身，而不是列出目录内的文件数据
-l : 长列表输出
--help： 查看更多参数的帮助文档
```

比如：

```bash
learn-linux@learnlinux-PC:~$ ls
Desktop  Documents  Downloads  examples.desktop  Music  Pictures  Public  Templates  Videos
learn-linux@learnlinux-PC:~$ ls -al
total 100
drwxr-xr-x 17 learn-linux learn-linux 4096 9月  17 10:11 .
drwxr-xr-x  3 root        root        4096 9月  16 13:42 ..
-rw-------  1 learn-linux learn-linux 1871 9月  17 09:01 .bash_history
-rw-r--r--  1 learn-linux learn-linux  220 9月  16 13:42 .bash_logout
-rw-r--r--  1 learn-linux learn-linux 3771 9月  16 13:42 .bashrc
drwx------ 14 learn-linux learn-linux 4096 9月  16 14:53 .cache
drwx------ 11 learn-linux learn-linux 4096 9月  16 13:48 .config
drwxr-xr-x  3 learn-linux learn-linux 4096 9月  16 13:51 Desktop
drwxr-xr-x  2 learn-linux learn-linux 4096 9月  16 13:47 Documents
drwxr-xr-x  2 learn-linux learn-linux 4096 9月  16 13:47 Downloads
-rw-r--r--  1 learn-linux learn-linux 8980 9月  16 13:42 examples.desktop
drwx------  3 learn-linux learn-linux 4096 9月  16 13:52 .gnupg
-rw-------  1 learn-linux learn-linux 2076 9月  16 14:43 .ICEauthority
drwx------  3 learn-linux learn-linux 4096 9月  16 13:46 .local
drwx------  5 learn-linux learn-linux 4096 9月  16 13:56 .mozilla
drwxr-xr-x  2 learn-linux learn-linux 4096 9月  16 13:47 Music
drwxr-xr-x  2 learn-linux learn-linux 4096 9月  16 13:47 Pictures
-rw-r--r--  1 learn-linux learn-linux  807 9月  16 13:42 .profile
drwxr-xr-x  2 learn-linux learn-linux 4096 9月  16 13:47 Public
drwx------  2 learn-linux learn-linux 4096 9月  16 13:52 .ssh
drwxr-xr-x  2 learn-linux learn-linux 4096 9月  16 13:47 Templates
drwx------  5 learn-linux learn-linux 4096 9月  16 14:01 .thunderbird
drwxr-xr-x  2 learn-linux learn-linux 4096 9月  16 13:47 Videos
```

`ls`命令输出的列表默认按照字母顺序，`ls`可以<font color="red">结合多个参数</font>使用，上面示例长列表中每一行都包括关于文件的下述信息:

- 文件类型，目录(d)、文件(-)，字符型文件(c)或块设备(b)；
- 文件权限(参见之后章节)；
- 文件的硬链接综述；
- 文件属主的用户名；
- 文件属组的组名；
- 文件大小（默认字节为单位）
- 文件上一次修改时间
- 文件名/目录名

同时，`ls`可以和通配符联用，筛选输出列表

- `?` 代表1个字符；
- `*` 代表0或多个字符
- `[a-z]` 指定字母范围为a-z

比如：

```bash
ls -ld D*
drwxr-xr-x 3 learn-linux learn-linux 4096 9月  16 13:51 Desktop
drwxr-xr-x 2 learn-linux learn-linux 4096 9月  16 13:47 Documents
drwxr-xr-x 2 learn-linux learn-linux 4096 9月  16 13:47 Downloads
ls -ld D[h-z]*
drwxr-xr-x 2 learn-linux learn-linux 4096 9月  16 13:47 Documents
drwxr-xr-x 2 learn-linux learn-linux 4096 9月  16 13:47 Downloads
```

### cd 切换目录

```bash
cd destination
# e.g.
## 绝对路径
cd /usr/bin/
## 相对路径
cd ../../etc
## 回到家目录（一键回城）
cd ~
cd
```

`destination`为目标目录路径，可使用绝对路径或相对路径表示

### pwd 打印当前目录

在使用Linux系统进行命令行操作时，需要经常在不同目录间切换，使用`pwd`可以迅速显示当前工作目录。

```bash
learn-linux@learnlinux-PC:~$ cd /usr/bin
learn-linux@learnlinux-PC:/usr/bin$ pwd
/usr/bin
```

### mkdir 创建新目录

```bash
mkdir [-p] New_Dir
-p : 创建多级目录
```

### rmdir 删除空目录

```bash
rmdir [-p] Dir
-p ： 删除多级空目录
```

当目录不为空时，上诉命令会运行失败，可使用下文的`rm`命令进行递归删除。

### tree 展示目录

`tree`可能没有默认安装在Linux发行版中，在后续的教程中，我们将学习如何安装软件。这里为了展示方面，只显示3层子目录

```bash
learn-linux@learnlinux-PC:~$ tree -L 3
.
├── Desktop
│   └── vmware-tools-distrib
│       ├── bin
│       ├── caf
│       ├── doc
│       ├── etc
│       ├── FILES
│       ├── INSTALL
│       ├── installer
│       ├── lib
│       ├── vgauth
│       ├── vmware-install.pl
│       └── vmware-install.real.pl
├── Documents
├── Downloads
├── examples.desktop
├── Music
├── Pictures
├── Public
├── Templates
└── Videos
```