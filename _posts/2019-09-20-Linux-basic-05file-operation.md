---
layout: post
title: 'Linux基础-05文件操作'
date: 2019-09-20
author: 柠檬菌
tags: Linux
---

> Linux常见文件操作

### touch 创建文件

touch可以创建空文件

```bash
touch test.txt
ls -l test.txt 
-rw-r--r-- 1 learn-linux learn-linux 0 9月  17 10:56 test.txt
```

同时touch还可用来改变文件的修改时间，而不改变文件内容

```bash
ls -l examples.desktop 
-rw-r--r-- 1 learn-linux learn-linux 8980 9月  16 13:42 examples.desktop
touch examples.desktop 
ls -l examples.desktop 
-rw-r--r-- 1 learn-linux learn-linux 8980 9月  17 10:59 examples.desktop
```

### mv 移动文件或改名

```bash
mv [-fiu] 源文件 目标文件
-f: force强制的意思，如果目标文件已经存在，不会询问而直接覆盖
-i: 若目标文件已经存在，询问是否覆盖
-u: 若目标文件已经存在，仅在复制文件比目标文件新的情况下覆盖
```

```bash
mv test.txt Desktop/
ls -l Desktop/test.txt 
-rw-r--r-- 1 learn-linux learn-linux 0 9月  17 10:56 Desktop/test.txt
mv Desktop/test.txt Desktop/test2.txt
ls -l Desktop/test*
-rw-r--r-- 1 learn-linux learn-linux 0 9月  17 10:56 Desktop/test2.txt
```

`mv`命令可以和通配符联用批量移动文件。

### cp 复制文件/目录

```bash
cp [-f] 源文件 目标文件
-r：递归复制目录
```

```bash
cp Desktop/test2.txt Desktop/test.txt
ls -l Desktop/test*
-rw-r--r-- 1 learn-linux learn-linux 0 9月  17 10:56 Desktop/test2.txt
-rw-r--r-- 1 learn-linux learn-linux 0 9月  17 11:09 Desktop/test.txt

learn-linux@learnlinux-PC:~$ cp -r Downloads/ Desktop/
learn-linux@learnlinux-PC:~$ ls -l Desktop/
total 8
drwxr-xr-x 2 learn-linux learn-linux 4096 9月  17 11:10 Downloads
-rw-r--r-- 1 learn-linux learn-linux    0 9月  17 10:56 test2.txt
-rw-r--r-- 1 learn-linux learn-linux    0 9月  17 11:09 test.txt
drwxr-xr-x 9 learn-linux learn-linux 4096 8月  26  2016 vmware-tools-distrib
```

同样，`cp`命令也可和通配符联用进行批量复制。

### rm 删除文件/目录

```bash
rm [-fir] 文件或目录
-f ： 强制删除
-i ： 互动模式，删除前进行询问
-r ： 递归删除目录
```

<font color="red">慎用rm，建议使用时加入-i 参数</font>，命令行删除后没有回收站，除非使用磁盘恢复工具。

删库跑路系列:

![http://ww3.sinaimg.cn/large/9150e4e5gy1fqhmq0ratwj206c0f8q38.jpg](http://ww3.sinaimg.cn/large/9150e4e5gy1fqhmq0ratwj206c0f8q38.jpg)

### ln 链接文件

如果需要维护同一文件的两份或多份副本，除了保存多份单独的物理文件副本之外， 还可以采用保存一份物理文件副本，多个虚拟副本的方法。虚拟副本就称为链接。链接是指向文件真实位置的占位符。Linux有两种不同类型的文件链接：

- 硬链接：在Linux系统中，内核为每一个文件分配一个Inode(索引节点)。Linux中多个文件名指向同一索引节点就是硬链接。其会创建读你的虚拟文件，包含原始文件的信息和位置，从根本上是同一个文件。
- 软链接：又称符号链接，类似Windows下的快捷方式

```bash
## 创建软链接
ln -s test.txt sl_test.txt
## 创建硬链接
ln test.txt hl_test.txt
ls -l *.txt
-rw-r--r-- 2 learn-linux learn-linux 19 9月  17 11:41 hl_test.txt
lrwxrwxrwx 1 learn-linux learn-linux  8 9月  17 11:40 sl_test.txt -> test.txt
-rw-r--r-- 2 learn-linux learn-linux 19 9月  17 11:41 test.txt
# 查看文件inode
ls -i *.txt
921537 hl_test.txt  921263 sl_test.txt  921537 test.txt
```

![硬链接vs软链接](https://ae01.alicdn.com/kf/H974114bc58f74b7c889f545a4cb6bf756.png)

### file 查看文件类型

在查看文件内容之前，应该先了解文件类型，如果打开二进制文件，屏幕会输出一堆乱码。此时`file`命令就非常有用了。

```bash
file test.txt
test.txt: ASCII text

file sl_test.txt 
sl_test.txt: symbolic link to test.txt

file /bin/ls
/bin/ls: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=9567f9a28e66f4d7ec4baf31cfbf68d0410f0ae6, stripped
```

### 查看整个文件

- cat

```bash
cat [-n] file_name
-n : 显示行号
```

`cat`将文件中的文本打印输出到屏幕上，对于大型文件而言，文本会一晃而过，不能进行有效控制。下面两个命令可以有效地解决这个问题。

- more

```bash
more file_name
```

`more`是个分页工具，一次显示一屏文本，通过`空格Space`或`回车Enter`向下翻页，`q`退出文档

- less

`less`用法和`more`类似，其还可以通过上下键滚动，还可以查找文档。

### 查看部分文件

- tail

查看文件的“尾部”

```bash
tail [-nf] file_name
-n: 指定显示末尾行数，默认为最后10行
-f: 允许在其他进程使用该文件时查看文件的内容，查看日志时非常有用
```

- head

查看文件的“头部”


```bash
# 默认查看文件前10行
head file_name
# 查看文件前5行
head -5  file_name
```

### Tips

- TAB自动补全

文件名太长或是命令记忆模糊，可以双击TAB进行补全，可以极大提高工作效率~

- 方向键`↑` `↓`可以调用上次输入命令，或者使用`history`查看历史命令
- 命令参数记不住怎么办？使用`man`命令查看手册，妈妈再也不用担心我的学习了~

```bash
man ls
```

![ls帮助手册](https://ae01.alicdn.com/kf/Hfaf46b4e37524d1685a6f89259c5fbbah.png)

使用空格翻页，或回车逐行查看，`q`退出手册帮助。

而且大多数命令支持`--help`或`-help`参数，显示帮助信息。
