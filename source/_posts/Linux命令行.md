---
title: Linux命令行
tags: [Linux学习]
categories: [Linux]
index_img: /Photo/Blog_Photo/index/Linux命令行.jpg
banner_img: /Photo/Blog_Photo/Banner/Linux命令行.jpg
typora-root-url: Linux命令行
date: 2022-09-05 12:09
---

# 初识shell

虽然我们已经安装好了系统，但是光会安装不会操作是不够的。我们还要像玩手机一样熟悉并记忆操作方法。

shell是系统的用户界面,提供了用户与内核进行交互操作的一种接口。它接收用户输入的命令并把它送入内核去执行。实际上shell是一个命令解释器，它解释用户输入的命令并且把用户的意图传达给内核。 （可以理解为用户与内核之间的翻译官角色）

![Shell是什么](image-20220712181206947.png)

我们可以使用shell实现对Linux系统单的大部分管理，例如：

1. 文件管理
2. 用户管理
3. 权限管理
4. 磁盘管理
5. 软件管理
6. 网络管理

使用shell的两种方式

- 交互式命令行
  - 默认等待用户输入命令，输入一行回车后执行一行命令
  - 效率低 适合少量的工作

- shell脚本

  - 将需要执行的命令和逻辑判断语句都写入一个文件中，一起运行

  - 效率高 适合完成复杂，重复性工作

# bash shell提示符

登录Linux系统之后，默认进入交互式的命令行界面，在光标前边会出现提示符

```python
[root@localhost ~]#
[用户名@主机名 目录名]权限标识
```

- 用户名

  - 当前登录的用户

  

- 主机名

  - 当前这台主机的名字，默认叫 localhost

    

- 目录名

  - 当前光标所在的目录

  - 当前光标所在的目录

    

- 权限标识

  - 超级管理员权限就表示为 #
  - 普通用户标识为 $

这个提示符格式被 $PS1 控制，我们可以查看这个变量

```python
[root@localhost ~]# echo $PS1
[\u@\h \W]\$
# \u表示是用户名 \h表示的是主机名 \W表示的当前所在目录 \$是权限标识
[root@localhost ~]# export PS1="{\u@\h}\W \$"
{root@localhost}~ $
# 可以通过export命令修改PS1变量，让提示符可以根据你的习惯变化
```

# shell语法

命令 选项 参数

```shell
[root@localhost ~]# cal --year -m 2020
```

- 命令
  - cal 是命令，用于查看日历

- 选项
  - --year 是选项，表示显示一整年，这个是一个长选项，也就是单词都拼全了，需要两条 - 符号
  - -m 是短选项，是首字母，表示每个星期的星期一作为第一天
  - 对于有些命令而言，可以不写选项，这样命令会有个默认的行为
  - 短选项可以多个合并在一起，比如上面的命令可以写成 -ym 其中y是year简写，可以和m写在 一起，而长选项不支持写在一起

- 参数
  - 2020 是参数，参数是命令作用的对象，表示查看的是2020年的日历

我们也可以查看这个命令的所有选项

```vbscript
[root@localhost ~]# cal --help
用法：
cal [选项] [[[日] 月] 年]
选项：
-1, --one 只显示当前月份(默认)
-3, --three 显示上个月、当月和下个月
-s, --sunday 周日作为一周第一天
-m, --monday 周一用为一周第一天
-j, --julian 输出儒略日
-y, --year 输出整年
-V, --version 显示版本信息并退出
-h, --help 显示此帮助并退出
```

# 常用命令

Linux的常见命令比较多，这边只列出初学者最常用的部分命令，大家可以根据命令意思去进行练习。

注意Linux会准确的识别出命令的大小写，所以大家需要注意大小写的问题。命令选项和参数之间是用空格进行分隔，请大家在输入的时候注意不要缺失空格。

学习Linux最重要的就是以下三个方面

```
1. 命令的积累
2. 原理的掌握
3. 大量的实战
```

下面就是开始第一步，积累基础的命令

## ls

用于显示指定工作目录下之内容（列出目前工作目录所含之文件及子目录)

```
 ls [-alrtAFR] [name...]
```

### 选项

- -a：显示所有文件及目录 (.开头的隐藏文件也会列出)
- -l：除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
- -r：将文件以相反次序显示(原定依英文字母次序)
- -t：将文件依建立时间之先后次序列出
- -A：同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)
- -F：在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"
- -R：若目录下有文件，则以下之文件亦皆依序列出
- -h：将显示出来的文件大小以合适的单位显示出来

### 实例

- 查看当前目录下的文件

```python
[root@localhost ~]# ls
```

- 查看根目录下的文件，查看/usr目录下的文件

```python
[root@localhost ~]# ls /
[root@localhost ~]# ls /usr
```

- 查看当前目录下所有文件，包括隐藏文件

```python
[root@localhost ~]# ls -a
```

- 查看当前目录下文件详情，包括隐藏文件

```python
 [root@localhost ~]# ls -lha
```

- 查看当前目录下的文件，并且显示出目录，文件，程序的区别

```python
[root@localhost ~]# ls -F
anaconda-ks.cfg dirb/ dird/ file2 file4 ping*
dira/ dirc/ dire/ file1 file3 file5
# 可以看到普通文件只有文件名，可执行文件后面带*，文件夹后面带/
```

- 查看当前目录下的文件，如果有文件夹，那么将文件夹中的文件也显示出来

```python
[root@localhost ~]# ls -FR
# dir这是一个目录，在这个目录下的文件也全部显示出来
[root@localhost ~]# ls -FRl
# 显示详细的信息
```

### 扩展知识

```python
[root@localhost ~]# ls -ahl
总用量 24K
dr-xr-x---. 3 root root 139 4月 2 14:00 .
dr-xr-xr-x. 17 root root 224 6月 21 2020 ..
-rw-------. 1 root root 128 4月 2 09:37 .bash_history
-rw-r--r--. 1 root root 18 12月 29 2013 .bash_logout
-rw-r--r--. 1 root root 176 12月 29 2013 .bash_profile
-rw-r--r--. 1 root root 176 12月 29 2013 .bashrc
-rw-r--r--. 1 root root 100 12月 29 2013 .cshrc
drwxr-xr-x. 2 root root 32 4月 2 14:00 dir
-rw-r--r--. 1 root root 129 12月 29 2013 .tcshrc
-rw-r--r--. 1 root root 0 4月 2 14:00 test.txt
```

1. 第一列共10位，第1位表示文档类型， d 表示目录， - 表示文件， l 表示链接文件， d 表示可随机 存取的设备，如U盘等， c 表示一次性读取设备，如鼠标、键盘等。后9位，依次对应三种身份所拥 有的权限，身份顺序为：owner、group、others，权限顺序为：readable、writable、 excutable。如： -r-xr-x--- 的含义为**当前文档是一个文件，拥有者可读、可执行，同一个群组下的 用户，可读、可写，其他人没有任何权限。**
2. 第二列表示链接数，表示有多少个文件链接到inode号码。
3. 第三列表示拥有者
4. 第四列表示所属群组
5. 第五列表示文档容量大小，单位字节
6. 第六列表示文档最后修改时间，注意不是文档的创建时间哦
7. 第七列表示文档名称。以点(.)开头的是隐藏文档

## cd

用于切换当前工作目录

```python
 cd [dirName]
```

### 实例

- 跳转到 /usr/bin 目录下

```python
[root@localhost ~]# cd /usr/bin
```

- 跳到自己的 home 目录

```python
[root@localhost bin]# cd ~
```

- 跳到目前目录的上一层

```python
[root@localhost ~]# cd ..
```

- 跳转到之前所在的位置

```python
[root@localhost ~]# cd -
```

## pwd

显示工作目录

```
pwd [-LP]
```

- -L 打印 $PWD 变量的值，如果它命名了当前的工作目录
- -P 打印当前的物理路径，不带有任何的符号链接

默认情况下， pwd 的行为和带 -L 选项一致

```python
[root@localhost ~]# export PWD=/usr/bin
[root@localhost bin]#
# 修改了$PWD变量，会导致当前光标的路径发生变化,只是显示切换了，但是实际目录没有修改
```

## clear

用于清除屏幕

使用快捷键 ctrl+l 也可以实现一样的效果

## echo

用于字符串的输出

```python
echo [-neE] 字符串
```

### 选项

- -n：不输出行尾的换行符

- -e：允许对下面列出的加反斜线转义的字符进行解释
  - \  反斜线
  - \a  报警符(BEL)
  - \b  退格符
  - \c  禁止尾随的换行符
  - \f  换页符
  - \n  换行符
  - \r  回车符
  - \t  水平制表符
  - \v  纵向制表符

- -E 禁止对在STRINGs中的那些序列进行解释

### 实例

- 显示出 hello world

```python
[root@localhost ~]# echo "hello world"
```

- 用两行显示出 hello world

```python
[root@localhost ~]# echo -e "hello\nworld"
```

- 输出 hello world 的时候让系统发出警报音

```
[root@localhost ~]# echo -e "hello\aworld"
```

# 系统命令

## poweroff

用于关闭计算器并切断电源

```python
poweroff [-n] [-w] [-d] [-f] [-i] [-h]
```

### 选项

- -n: 在关机前不做将记忆体资料写回硬盘的动作
- -w: 并不会真的关机，只是把记录写到 /var/log/wtmp 档案里
- -d: 不把记录写到 /var/log/wtmp 文件里
- -i: 在关机之前先把所有网络相关的装置先停止
- -p: 关闭操作系统之前将系统中所有的硬件设置为备用模式。

## reboot

用来重新启动计算机

```
用来重新启动计算机
```

### 选项

- -n: 在关机前不做将记忆体资料写回硬盘的动作
- -w: 并不会真的关机，只是把记录写到 /var/log/wtmp 档案里
- -d: 不把记录写到 /var/log/wtmp 文件里（-n 这个参数包含了 -d）
- -f: 强迫重开机，不呼叫 shutdown 这个指令
- -i: 在重开机之前先把所有网络相关的装置先停止

## whoami

用于显示自身用户名称

```python
[root@localhost ~]# whoami
root
```

# 快捷键

| 快捷键 | 作用                     |
| ------ | ------------------------ |
| ^C     | 终止前台运行的程序       |
| ^C     | 退出 等价exit            |
| ^L     | 清屏                     |
| ^A     | 光标移动到命令行的最前端 |
| ^E     | 光标移动到命令行的后端   |
| ^U     | 删除光标前所有字符       |
| ^K     | 删除光标后所有字符       |
| ^K     | 搜索历史命令，利用关键词 |

# 帮助命令

## history

```
history [n] n为数字，列出最近的n条命令
```

### 选项

- -c：将目前shell中的所有history命令消除
- -a：将目前新增的命令写入histfiles, 默认写入 ~/.bash_history
- -r：将histfiles内容读入到目前shell的history记忆中
- -w：将目前history记忆的内容写入到histfiles

### 实例

- 将history的内容写入一个新的文件中

```
[root@localhost ~]# history -w histfiles.txt
```

- 清空所有的history记录，注意并不清空 ~/.bash_history 文件

```
[root@localhos t ~]# history -c
```

- 使用 ! 执行历史命令。 
- ! number 执行第几条命令 
- ! command 从最近的命令查到以 command 开头的命令执行 
- ! !执行上一条

```python
[root@localhost ~]# history
    1 history
    2 cat .bash_history
    3 ping -c 3 baidu.com
    4 history
[root@localhost ~]# !3
# 这里是执行第三条命令的意思
```

## help

显示命令的帮助信息

```python
help [-dms] [内置命令]
```

### 选项

- -d：输出每个主题的简短描述
- -m：以伪 man 手册的格式显示使用方法
- -s：为每一个匹配 PATTERN 模式的主题仅显示一个用法

#### 实例

- 查看echo的帮助信息

```python
[root@localhost ~]# help echo
```

## man

显示在线帮助手册页

```
man 需要帮助的命令或者文件
```

### 快捷键

| 按键      | 用途                               |
| --------- | ---------------------------------- |
| 空格键    | 向下翻一页                         |
| PaGe down | 向下翻一页                         |
| PaGe up   | 向上翻一页                         |
| home      | 直接前往首页                       |
| end       | 直接前往尾页                       |
| /         | 从上至下搜索某个关键词，如“/linux” |
| ?         | 从下至上搜索某个关键词，如“?linux” |
| n         | 定位到下一个搜索到的关键词         |
| N         | 定位到上一个搜索到的关键词         |
| q         | 退出帮助文档                       |

### 实例

- 查看echo的man手册

```python
[root@localhost ~]# man echo
ECHO(1) General Commands Manual
			ECHO(1)
			
NAME(名称)
		echo - 显示一行文本
		
SYNOPSIS(总览)
			echo[OPTION]... [STRING]...

DESCRIPTION(描述)
	允许在标准输出上显示STRING(s).

-n 不输出行尾的换行符.

-e 允许对下面列出的加反斜线转义的字符进行解释.

-E 禁止对在STRINGs中的那些序列进行解释.
```

## alias

用于设置指令的别名

- 查看系统当前的别名

```shell
[root@localhost ~]# alias # 查看系统当前的别名
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --
show-tilde'
[root@localhost ~]# ll
总用量 4
-rw-------. 1 root root 1241 8月 22 2018 anaconda-ks.cfg
drwxr-xr-x. 2 root root 19 8月 21 12:15 home
[root@xwz ~]# type -a ls # 查看命令类型
ls 是 `ls --color=auto' 的别名
ls 是 /usr/bin/ls
```

- 修改别名，比如使用wl来查看IP地址相关信息

```shell
[root@localhost ~]# alias wl='ip address'
[root@localhost ~]# wl
```

为了让别名永久生效，可以讲修改别名的命令写入 bashrc 文件，这个文件中的命令会在每次登陆 命令行的时候执行

```python
[root@localhost ~]# echo "alias wl='ip address'" >> /etc/bashrc
```
