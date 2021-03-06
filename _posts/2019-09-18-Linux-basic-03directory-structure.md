---
layout: post
title: 'Linux基础-03目录结构'
date: 2019-09-18
author: 柠檬菌
tags: Linux
---

> Linux核心思想：一切皆文件，统一的名字空间 (unified namespace)：表现形式是文件系统 (filesystem) 的路径；统一的访问接口 (unified interface)：表现形式是 read/write 等标准函数。 -- 知乎用户Rio

### Linux系统目录结构

熟悉Windows操作系统的人可能会注意到，Linux在路径名中不使用驱动器盘符，即没有所谓的C盘，D盘；Linux只包含一个根（root）目录的树状结构。

而且Linux使用正斜线`/`划分目录，而Windows使用反斜线`\`，Linux中的反斜线用来标识转义字符。

下图为Linux树状目录结构示意图：

![filesystem-tree.png](https://ae01.alicdn.com/kf/H3bd73bdbc67d49e9932584f84b44821et.png)

![file-system](https://ae01.alicdn.com/kf/Hdbb5ae099d0242fe826aea75faba8dbfL.png)

其中：

`/bin`：bin是Binary的缩写，存放着最经常使用的命令；

`/boot`：存放启动Linux的核心文件，包括一些链接文件及镜像文件；

`/dev`：dev是Device的缩写，存放Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的；

`/etc`：系统配置文件目录；

`/home`：用户的主目录，Linux中每个用户都有自己的一个目录，通常该目录名为用户登录账号名；

`/lib`：存放系统最基本的动态链接共享库，作用类似于Windows中的DLL文件。几乎所有的应用程序都需要用到这些共享库；

`/root`：系统管理员目录，超级权限的用户主目录；

`/tmp`：存放临时文件；

`/usr`：用户的很多应用程序和文件都放在该目录下，类似Windows下的Program files目录；

`/var`： 可变目录，存放经常变化的文件，比如日志文件。

### 认识你的机器

#### 系统版本和处理器架构

```bash
uname -a
Linux learnlinux-PC 5.0.0-23-generic #24~18.04.1-Ubuntu SMP Mon Jul 29 16:12:28 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```

- 内核名称： Linux
- 主机名：learnlinux-PC
- 操作系统发行编号：5.0.0-23-generic
- 操作系统版本：#24~18.04.1-Ubuntu SMP Mon Jul 29 16:12:28 UTC 2019
- 处理器类型：x86_64
- 硬件平台：x86_64
- 操作系统名称：GNU/Linux

#### 内存大小

```bash
free -m
              total        used        free      shared  buff/cache   available
Mem:           1966        1160         180          14         625         624
Swap:           947         243         703
```

系统总内存为：1966Mb 近似为 2Gb，和创建虚拟机时相同

#### 硬盘空间

```bash
df -h

Filesystem      Size  Used Avail Use% Mounted on
udev            960M     0  960M   0% /dev
tmpfs           197M  1.8M  195M   1% /run
/dev/sda1        20G  6.3G   13G  34% /
tmpfs           984M     0  984M   0% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           984M     0  984M   0% /sys/fs/cgroup
```

显示所有挂载设备的当前磁盘空间使用情况

使用`df`命令查看哪块磁盘存储空间快没了后，使用`du`命令显示指定目录(默认为当前目录)的磁盘使用情况

```bash
du -h --max-depth=1

4.0K	./Public
23M	./.cache
8.0K	./Desktop
164K	./.config
4.0K	./Downloads
4.0K	./Music
4.0K	./Videos
1.5M	./.local
4.0K	./Pictures
4.0K	./Templates
13M	./.thunderbird
19M	./.mozilla
4.0K	./.ssh
16K	./.gnupg
4.0K	./Documents
56M	.
```

`-h`表示`human read`，以K, M, G作为单位，`--max-depth=1`表示当前目录的各个子目录所使用的空间

#### CPU信息

```bash
less /etc/cpuinfo
# 或者
lscpu

Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              2
On-line CPU(s) list: 0,1
Thread(s) per core:  1
Core(s) per socket:  2
Socket(s):           1
NUMA node(s):        1
Vendor ID:           GenuineIntel
CPU family:          6
Model:               94
Model name:          Intel(R) Core(TM) i7-6700 CPU @ 3.40GHz
Stepping:            3
CPU MHz:             3408.109
BogoMIPS:            6816.21
Virtualization:      VT-x
Hypervisor vendor:   VMware
Virtualization type: full
L1d cache:           32K
L1i cache:           32K
L2 cache:            256K
L3 cache:            8192K
NUMA node0 CPU(s):   0,1
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts nopl xtopology tsc_reliable nonstop_tsc cpuid aperfmperf pni pclmulqdq vmx ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch cpuid_fault invpcid_single pti tpr_shadow vnmi ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 invpcid rtm rdseed adx smap xsaveopt dtherm ida arat pln pts hwp hwp_notify hwp_act_window hwp_epp
```