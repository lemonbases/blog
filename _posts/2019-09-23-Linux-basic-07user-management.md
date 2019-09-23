---
layout: post
title: 'Linux基础-07用户管理'
date: 2019-09-23
author: 柠檬菌
tags: Linux
---

Linux系统是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须向系统管理员申请一个账号，然后以这个账号的身份进入系统。每个用户账号都拥有一个唯一的用户名和各自的密码。用户在登录时输入正确的用户名和密码，就可以进入系统和自己的主目录。

### 用户管理

- 添加新用户

可以使用`useradd`或`adduser`创建新用户。注意的是，创建用户需要`管理员权限`，可在下面命令先加`sudo`，以管理员权限运行。

```bash
useradd 参数与选项
-c : comment 指定一段注释性描述
-d : 指定用户主目录，如果该目录不存在，则同时使用-m选项，可以创建主目录
-g : 指定用户所属的用户组
-G : 指定用户所属的附加组
-s : 指定用户使用的shell类型
-m : 创建用户的HOME目录
-M : 不创建用户的HOME目录
-p : 为用户账户指定默认密码
......
# 创建用户并指定其shell类型为/bin/sh，属于group用户组，也同时属于adm和root，其用户主目录为/home/user
useradd -d /home/user -m -s /bin/sh -g group -G adm,root user

ls -al /home/user

drwxr-xr-x 2 user user 4096 9月  18 10:47 .
drwxr-xr-x 4 root root 4096 9月  18 10:47 ..
-rw-r--r-- 1 user user  220 4月   5  2018 .bash_logout
-rw-r--r-- 1 user user 3771 4月   5  2018 .bashrc
-rw-r--r-- 1 user user 8980 4月  16  2018 examples.desktop
-rw-r--r-- 1 user user  807 4月   5  2018 .profile
```

可以发现`user`目录中有一些默认文件，这些是从`/etc/skel`中拷贝而来，管理员可以通过更改`/etc/skel`的文件，来作为新用户HOME目录的模板。但此时用户没有设置密码，设置密码需要用`passwd`命令，

```bash
passwd 选项 用户名
-l : 锁定密码，即禁用账号
-u : 解锁密码
-d : 使用户无密码
-f : 强迫用户下次登录时修改密码

# 超级用户可以 passwd 用户名直接指定任何用户的密码
passwd user

Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
```

而`adduser`是一个perl脚本，在使用时比`useradd`更友好。

```bash
adduser user2

Adding user `user2' ...
Adding new group `user2' (1002) ...
Adding new user `user2' (1002) with group `user2' ...
Creating home directory `/home/user2' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for user2
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n]
```

这时已经在/home下面创建与用户同名的目录，而且密码也已经设置好了。

- 删除用户

`userdel`命令只删除`/etc/passwd`文件中用户信息，及用户不能再通过该账户登录，但不会删除系统中属于该账户的任何文件。如果删除用户的HOME目录及邮件目录，可以加上`-r`参数。

```bash
# 删除用户user，但不删除其家目录及文件;
userdel user
# 删除用户user2，其家目录及文件一并删除;
userdel -r user2
ls -l /home

drwxr-xr-x 17 learn-linux learn-linux 4096 9月  18 13:30 learn-linux
drwxr-xr-x  2        1001        1001 4096 9月  18 10:47 user
```

- 修改用户

`usermod`是用户账户修改工具中最强大的一个，其参数和useradd命令的参数大部分一样，还有一些参数也比较实用：

```bash
# 将user加入组adm中
usermod -G adm user
# 修改user的用户名为user2
usermod -l user2 user
# 锁定账户user2
usermod -L user2
# 解除账户user2的锁定
usermod -U user2
```

### 用户组管理

Linux是多用户系统，当涉及到共享资源的一组用户时，就有了组(group)的概念。组权限会允许多个用户对系统中的对象共享一组共用的权限，正如我们在上一节文件权限中介绍的。

用户账户信息存放在`/etc/passwd`中，而组信息存放在`/etc/group`。

```bash
head -n 5 /etc/group

root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,learn-linux
```

由组名、组密码、GID（group ID）、组成员四个字段组成。

- 创建新组

```bash
groupadd share
```

创建新组时，默认没有用户分配到该组，可以使用上文介绍的`usermod`来将用户增加到新组

```bash
#将用户user1, user2加入到组share中
usermod -G share user1
usermod -G share user2

tail -n 1 /etc/group

share:x:1004:user1,user2
```

如果将用户从组中移除，可以使用`gpasswd`命令，其可以将用户加入到组中：

```bash
gpasswd [option] GROUP
-a : 将用户加入到组中
-d : 将用户移除组

# 将user1移除share组
gpasswd -d user1 share

tail -1 /etc/group

share:x:1004:user2
```

- 修改组

`groupmod`可以修改已有组的GID（-g）或组名（-n）

```bash
# 将组share重命名为sharing
groupmod -n sharing share
tail -l /etc/group

sharing:x:1004:user2
```

