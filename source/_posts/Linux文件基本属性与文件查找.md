---
title: Linux文件基本属性与文件查找
tags: [Linux文件管理]
categories: [Linux]
index_img: /Photo/Blog_Photo/index/Linux文件基本属性与文件查找.jpg
banner_img: /Photo/Blog_Photo/Banner/Linux文件基本属性与文件查找.jpg
typora-root-url: Linux文件基本属性与文件查找
date: 2022-10-03 21:21:22
---

# 文件时间

任何一个操作系统都有时间的概念，时间的概念主要用于对文件和系统中发生的时间进行记录，在Linux 中，可以使用stat查看Linux系统中文件的时间

## stat

用于显示文件时间和 inode 内容，inode相关的知识会在后面的磁盘管理章节详细讲解，这边主要来看 文件的时间

```shell
stat [选项]... 文件...
```

### 实例

- stat查看文件时间，这边为了我们方便看得懂，建议改为英文系统环境

```shell
[root@localhost ~]# export LANG="en_US.UTF-8"
# 改回中文是LANG="zh_CN.UTF-8"
[root@localhost ~]# stat anaconda-ks.cfg
File: ‘anaconda-ks.cfg’
Size: 1241 Blocks: 8 IO Block: 4096 regular file
Device: fd00h/64768d Inode: 33574979 Links: 1
Access: (0600/-rw-------) Uid: ( 0/ root) Gid: ( 0/ root)
Context: system_u:object_r:admin_home_t:s0
Access: 2021-04-04 17:54:09.700844151 +0800
Modify: 2021-04-04 16:53:30.524854041 +0800
Change: 2021-04-04 16:53:30.524854041 +0800
Birth: -
```

- Access：访问时间，也叫atime
  - 当文件被访问的时候，这个时间就会发生改变
  - Linux文件运行的时候查看文件又频繁数量又大，如果每次atime发生变化的时候都记入硬 盘，或造成很大的压力。RHEL6开始relatime，atime延迟修改，必须满足其中一个条件：
    - 自上次atime修改后，已达到86400秒
    - 发生写操作时
- Modify：修改时间，也叫mtime
  - 当文件内容发生变化的时候，这个时间就会发生改变
- Change：改变时间，也叫ctime
  - 当文件状态被改变的时候，这个时间就会发生修改

# 文件类型

Linux系统和Windows系统有很大的区别，Windows系统查看文件的后缀名就可以知道这个是什么类型 的文件，比如： test.jpg 这个是一个图片，如果你在windows上双击打开，就会使用支持查看图片的 软件打开。

Linux系统就根本不看文件的后缀名，你认为这个是什么文件，你就使用什么工具打开这个文件，如果打 开错误，就会报错，看下面的案例

```shell
[root@localhost ~]# cat file
cat: file: Is a directory
```

## 方法一：ls

使用ls可以查看当前目录下有哪些文件，我们会发现文件夹和文件的颜色并不一样，所以我们可以简单 的通过颜色来进行判断，不过这种判断的方式并不准确，因为不同的Linux发行套件颜色的标准并不一 样，不同的远程管理工具对颜色的理解也有偏差，比如可能把蓝色显示为淡蓝色，而淡蓝色又显示成其 他颜色。所以最推荐的做法是通过 ls -l 查看第一个字母：

- -普通文件(文本文档，二进制文件，压缩文件，电影，图片。。。)
- d目录文件(蓝色)
- b块设备文件(块设备)存储设备硬盘，U盘 /dev/sda,/dev/sda1
- c字符设备文件(字符设备)打印机，终端 /dev/tty1,/dev/zero
- s套接字文件
- p管道文件
- p管道文件

```shell
[root@localhost ~]# type ll
ll 是 `ls -l --color=auto' 的别名
[root@localhost ~]# ll -d /etc/hosts /bin/ls /home /dev/sda /dev/tty1
/etc/grub2.cfg /dev/log /run/dmeventd-client
-rwxr-xr-x. 1 root root 117680 10月 31 2018 /bin/ls
srw-rw-rw-. 1 root root 0 4月 4 16:54 /dev/log
brw-rw----. 1 root disk 8, 0 4月 4 16:54 /dev/sda
crw--w----. 1 root tty 4, 1 4月 4 16:56 /dev/tty1
lrwxrwxrwx. 1 root root 22 4月 4 16:49 /etc/grub2.cfg ->
../boot/grub2/grub.cfg
-rw-r--r--. 1 root root 158 6月 7 2013 /etc/hosts
drwxr-xr-x. 2 root root 6 4月 11 2018 /home
prw-------. 1 root root 0 4月 4 16:54 /run/dmeventd-client
```

对于初学者而言，我们现在只要知道可以通过这样的方式查看文件的类型，并且能够知道 - 和 d 的意思 即可。后面在学习的过程中，会慢慢的将所有文件类型都掌握的。

## 方法二：file

file是专门用来查看文件的类型的命令，有时候也可以使用

```shell
[root@localhost ~]# file /etc/hosts
/etc/hosts: ASCII text
[root@localhost ~]# file /bin/ls
/bin/ls: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically
linked (uses shared libs), for GNU/Linux 2.6.32,
BuildID[sha1]=ceaf496f3aec08afced234f4f36330d3d13a657b, stripped
[root@localhost ~]# file /dev/sda
/dev/sda: block special
[root@localhost ~]# file /dev/tty1
/dev/tty1: character special
[root@localhost ~]# file /etc/grub2.cfg
/etc/grub2.cfg: symbolic link to `../boot/grub2/grub.cfg'
[root@localhost ~]# file /home
/home: directory
[root@localhost ~]# file /run/dmeventd-client
/run/dmeventd-client: fifo (named pipe)
```

## 方法三：stat

这个命令上面已经介绍过了，在输出结果中也是可以看到文件的类型

# 文件查找

在windows中可以在文件管理器中很方便的输入文件名查找文件，然而Linux的文件查找功能更加的方 便，并且功能更加的强大，现在就介绍三个用于查找文件的命令。

在这三种查找命令中功能最强大的是 find 命令，所以在学习的时候， which 和 locate 需要掌握， find 命令需要熟练掌握！

## which

用于查找文件

which指令会在环境变量 $PATH 设置的目录里查找符合条件的文件

```shell
which [文件...]
```

### 补充知识：环境变量

环境变量（environment variables）一般是指在操作系统中用来指定操作系统运行环境的一些参数， 如：临时文件夹位置和系统文件夹位置等。

简单的理解就是告诉操作系统在程序运行的时候，有一些默认的设置是什么。

比如上面我们修改了 LANG 变量，就是一个环境变量，会影响到显示的语言是中文还是英文。

比如在讲解 pwd 命令的时候，我们修改了 $PWD 变量，就影响了当前所处的文件夹。

在我们使用shell命令行输入命令的时候，其实每个命令都是有一个可执行文件去完成我们下达的任务， 这个可执行文件在操作系统中是分布在不同的文件夹中的，我们总不能每次执行的时候都要告诉操作系 统这个文件在哪里，那么就算是查看一个文件，我们都需要输入如下的命令：

```shell
[root@localhost ~]# /usr/bin/ls -lh
# 在Linux中，ls的可执行程序在/usr/bin目录下
```

这样就太麻烦了，所以就指定了一个环境变量 $PATH ，这个变量中有很多的目录地址，当我们执行命令 的时候，操作系统就会到这些目录中查找，是否存在你所输入的命令。如果有那么就会去执行。

```shell
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
```

如果你想让自己安装的某个软件可以在操作系统的任意位置直接输入文件名执行，那么你也可以把自定 义的目录加入到这个 $PATH 中

### 实例

- 查看ls命令的可执行文件在什么目录

```shell
[root@localhost ~]# which ls
alias ls='ls --color=auto'
/usr/bin/ls
# which会先告诉你ls其实是一个别名
# 然后显示出来ls所在的具体位置
```

**小知识：**我们在执行 ls 的时候，其实执行的是 ls --color=auto 这条命令，在显示文件的时候使用不 同的颜色表示不同的文件类型，如果我们想执行 ls 本体，而不想执行别名，我们可以输入 \ls 就可以 了，这样就不会有不同的颜色表示文件类型了。

- 查看poweroff在什么目录

```she
[root@localhost ~]# which poweroff
/usr/sbin/poweroff
```

## locate

用于查找符合条件的文件，他会去保存文件和目录名称的数据库内，查找合乎范本样式条件的文件或目 录

在centos7的最小化安装中，并没有自带locate命令，我们需要输入如下命令进行安装。

```shell
[root@localhost ~]# yum -y install mlocate
# 注意，在安装的时候需要确保虚拟机有网络
```

ocate的使用方式如下

```shell
locate [选项]... [范本样式]...
```

在使用locate之前，需要更新一下数据库，因为locate只会在数据库中查找文件所在的位置，所以locate 查找速度极快，缺点就是数据库更新并不是实时的，更新数据库有两种方式：

- 手动更新，输入 updatedb
- 默认情况下， updatedb 会每天自动执行一次
- 配置文件在/etc/updatedb.conf

### 选项

- -c：只输出找到的数量
- -c：只输出找到的数量
- -i：忽略大小写
- -r：使用基本正则表达式
- --regex：使用扩展正则表达式
- -d DBPATH：使用 DBPATH 指定的数据库，而不是默认数据库 /var/lib/mlocate/mlocate.db

### 实例

- 查找passwd文件所在的位置

```shell
[root@localhost ~]# updatedb
# 更新数据库并不是每次查找都需要，但是建议更新数据库来保证数据是最新的
[root@localhost ~]# locate passwd
```

- 查找ens33网卡配置文件所在的位置

```shell
查找ens33网卡配置文件所在的位置
```

关于正则表达式，我们会在后续文本三剑客中详细学习

## find

实时查找工具，通过遍历指定路径下的文件系统完成文件查找

工作特点:

- 查找速度略慢
- 精确查找
- 实时查找
- 可以满足多种条件匹配

```shell
find [选项] [路径] [查找条件 + 处理动作]
查找路径：指定具体目录路径，默认是当前文件夹
查找条件：指定的查找标准（文件名/大小/类型/权限等），默认是找出所有文件
处理动作：对符合条件的文件做什么操作，默认输出屏幕
```

### 查找条件

- 根据文件名查找

```shell
[root@localhost ~]# find /etc -name "ifcfg-ens33"
[root@localhost ~]# find /etc -iname "ifcfg-ens33" # 忽略大小写
[root@localhost ~]# find /etc -iname "ifcfg*"
```

- 按文件大小

```shell
[root@localhost ~]# find /etc -size +5M # 大于5M
[root@localhost ~]# find /etc -size 5M # 等于5M
[root@localhost ~]# find /etc -size -5M # 小于5M
[root@localhost ~]# find /etc -size +5M -ls # 找到的处理动作-ls
```

- 指定查找的目录深度

```shell
[root@localhost ~]# find / -maxdepth 3 -a -name "ifcfg-ens33" # 最大查找深度
# -a是同时满足，-o是或
[root@localhost ~]# find / -mindepth 3 -a -name "ifcfg-ens33" # 最小查找深度
```

- 按时间找

```shell
[root@localhost ~]# find /etc -mtime +5 # 修改时间超过5天
[root@localhost ~]# find /etc -mtime 5 # 修改时间等于5天
[root@localhost ~]# find /etc -mtime -5 # 修改时间5天以内
```

- 按照文件属主、属组找，文件的属主和属组，会在下一篇详细讲解。

```shell
[root@localhost ~]# find /home -user xwz # 属主是xwz的文件
[root@localhost ~]# find /home -group xwz
[root@localhost ~]# find /home -user xwz -group xwz
[root@localhost ~]# find /home -user xwz -a -group root
[root@localhost ~]# find /home -user xwz -o -group root
[root@localhost ~]# find /home -nouser # 没有属主的文件
[root@localhost ~]# find /home -nogroup # 没有属组的文件
```

按文件类型

```shell
[root@localhost ~]# find /dev -type d
```

- 按文件权限，文件权限会在下一篇详细讲解

```shell
[root@localhost ~]# find / -perm 644 -ls
[root@localhost ~]# find / -perm -644 -ls # 权限小于644的
[root@localhost ~]# find / -perm 4000 -ls
[root@localhost ~]# find / -perm -4000 -ls
```

- 按正则表达式

```shell
[root@localhost ~]# find /etc -regex '.*ifcfg-ens[0-9][0-9]'
# .* 任意多个字符
# [0-9] 任意一个数字
```

- 条件组合

  - -a：多个条件and并列

  - -o：多个条件or并列

  - -not：条件取反

## 处理动作

- ‐print：默认的处理动作，显示至屏幕
- ‐ls：类型于对查找到的文件执行 ls ‐l 命令
- ‐delete：删除查找到的文件
- ‐fls /path/to/somefile：查找到的所有文件的长格式信息保存至指定文件中
- *‐ok COMMAND {}*：对查找到的每个文件执行由COMMAND指定的命令，需要确认
- *‐exec COMMAND {} *：对查找到的每个文件执行由COMMAND指定的命令，不需要确认
- *‐exec COMMAND {} *：对查找到的每个文件执行由COMMAND指定的命令，不需要确认

下面的实例大家学习完后续用户权限管理之后，就可以完全看的懂了

### 实例

- 查找/var目录下属主为root，且属组为mail的所有文件或目录

```shell
[root@localhost ~]# find /var ‐user root ‐group mail
```

- 查找/usr目录下不属于root，bin或Hadoop的所有文件或目录

```shell
[root@localhost ~]# find /usr ‐not ‐user root ‐a ‐not ‐user bin ‐a ‐not ‐user
centos
[root@localhost ~]# find /usr ‐not \(‐user root ‐o ‐user bin ‐o ‐user hadoop\)
```

- 查找/etc目录下最近一周内容曾被修改过的文件或目录

```shell
[root@localhost ~]# find /etc/ ‐mtime ‐7
```

- 查找当前系统上没有属主或属组，且最近一周内曾被访问过的文件或目录

```shell
[root@localhost ~]# find / \(‐nouser ‐o ‐nogroup\) ‐a ‐atime ‐7
```

- 查找/etc目录下大于1M且类型为普通文件的所有文件或目录

```shell
[root@localhost ~]# find /etc ‐size +1M ‐type f
```

- 查找/etc目录下所有用户都没有写权限的文件

```shell
 [root@localhost ~]# find /etc ‐not ‐perm -222
```

- 查找/etc目录下至少一类用户没有执行权限的文件

```shell
[root@localhost ~]# find /etc ‐not ‐perm ‐111
```

- 查找/etc/init.d目录下，所有用户都执行权限，且其它用户写权限的文件

```shell
[root@localhost ~]#find /etc/init.d ‐perm ‐113
```
