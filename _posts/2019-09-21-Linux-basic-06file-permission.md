---
layout: post
title: 'Linux基础-06文件权限'
date: 2019-09-21
author: 柠檬菌
tags: Linux
---

>了解Linux系统下文件权限及管理

在[Linux-基础-04目录管理](https://lemonbases.github.io/2019/09/19/Linux-basic-04directory-operation.html)中，我们学习了`ls`命令可以用来显示目录内容。

```bash
ls -l

total 8
-rw-r--r-- 2 learn-linux learn-linux 19 9月  17 11:41 hl_test.txt
lrwxrwxrwx 1 learn-linux learn-linux  8 9月  17 11:40 sl_test.txt -> test.txt
-rw-r--r-- 2 learn-linux learn-linux 19 9月  17 11:41 test.txt
```

第一个字段描述文件和目录权限的编码，第一个字符表示对象类型：

- `-`代表文件
- `d`代表目录
- `l`代表链接
- `c`代表字符型设备
- `b`代表块设备
- `n`代表网络设备

之后有3组三字符的编码，共9位，每一组定义了3种访问权限：

- `r`代表可读
- `w`代表可写
- `x`代表可执行

若没有对应权限，用`-`表示。

![linux-file-permission.png](https://ae01.alicdn.com/kf/H88eb0f8a93234430abd2f83cb36c3bd9O.png)

权限的8进制表示：

**r ：4， w：2，x：1，-：0**

![linux-file-permission-8bits.png](https://ae01.alicdn.com/kf/H5e273635b251460a81ceb16e7568c51aM.png)

### chmod 权限更改

```bash
chmod options mode file
```

mode参数可以使用八进制模式或符号模式进行安全设置

#### 八进制模式

```bash
chmod 760 test.txt
ls -l test.txt

-rwxrw---- 2 learn-linux learn-linux 19 9月  17 11:41 test.txt
```

#### 符号模式

```bash
chmod [ugoa] [[+-=][rwx...]]
```

- u：文件所有者
- g：同组用户
- o：其它用户
- a：上述所有

后面的符号代表在现有权限基础上增加权限`+`, 移除权限`-`，或将权限设置为后面的值`=`

第三个符号代表设置的权限，常见的为`rwx`。

给`test.txt`文件的其它用户增加读权限。

```bash
chmod o+r test.txt
ls -l test.txt

-rwxrw-r-- 2 learn-linux learn-linux 19 9月  17 11:41 test.txt
```

### 改变所属关系

`chown`用于改变文件属主和属主，`chgrp`用于改变文件的属组，`-R`选项用于递归改变目录及其子文件

```bash
ls -al test.txt

-rwxrw-r-- 2 learn-linux learn-linux 19 9月  17 11:41 test.txt

#将test.txt文件属主更改为root，属组更改为root
chown root:root test.txt
ls -al test.txt

-rwxrw-r-- 2 root root 19 9月  17 11:41 test.txt

#将test.txt文件属组更改为test
chgrp learn-linux test.txt
ls -al test.txt

-rwxrw-r-- 2 root learn-linux 19 9月  17 11:41 test.txt
```

Tips：

当出现`Operation not permitted` error时，使用`sudo`提升权限。