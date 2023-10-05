---
title: Shell脚本编程
tags: [Shell脚本编程]
categories: [云计算]
index_img: /Photo/Blog_Photo/index/Shell脚本编程.jpg
banner_img: /Photo/Blog_Photo/Banner/Shell脚本编程.jpg
typora-root-url: Shell脚本编程
date: 2022-09-05 12:27
---

# Shell脚本编程

# 简介

- Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一 种程序设计语言。
- Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的 服务。(翻译官，帮你翻译命令给内核执行)

![相互之间的关系](image-20220812094238713.png)

- Linux 的 Shell 种类众多，常见的有：
  - Bourne Shell（/usr/bin/sh或/bin/sh）
  - Bourne Again Shell（/bin/bash）
  - C Shell（/usr/bin/csh）
  - K Shell（/usr/bin/ksh）
  - Shell for Root（/sbin/sh）
- 程序编程风格
  - 过程式：以指令为中心，数据服务于命令
  - 对象式：以数据为中心，命令服务于数据
  - shell是一种过程式编程
- 过程式编程
  - 顺序执行
  - 循环执行
  - 选择执行
- 编程语言分类
  - 编译型语言
  - 解释型语言（shell是一种解释型语言）

![两种分类语言之间的区别](image-20220812094530829.png)

运行脚本

给予执行权限，通过具体的文件路径指定文件执行

直接运行解释器，将脚本作为解释器程序的参数运行

bash退出状态码

范围是0-255

脚本中一旦遇到exit命令，脚本会立即终止，终止退出状态取决于exit命令后面的数字

如果未给脚本指定退出状态码，整个脚本的退出状态码取决于脚本中执行的最后一条命令的状态

# 变量

## 变量命名

- 命名只能使用英文字母，数字和下划线，首字母不能以数字开头
- 中间不能够有特殊字符，可以使用_下划线
- 不能使用标点符号
- 不能使用bash中的关键字

```bash
有效命名：
RUNOOB
LD_LIBRARY_PATH
_var
var2
无效命名：
?var=123
user*name=runoob
语句给变量赋值
for file in `ls /etc`
或
for file in $(ls /etc)
```

## 使用变量

```bash
定义变量：
your_name="eagles"
使用变量：
echo $your_name
echo ${your_name}
建议使用｛｝号作边界
for skill in Ada Coffe Action Java; do
echo "I am good at ${skill}Script"
done
如果使用$skillScript，则将会输出空值
```

## 只读变量

```bash
#!/bin/bash
myUrl="http://www.google.com"
readonly myUrl
myUrl="http://www.runoob.com"
执行脚本后，显示只读变量无法修改
```

## 删除变量

```bash
#!/bin/sh
myUrl="http://www.runoob.com"
unset myUrl
echo $myUrl
```

## 变量种类

- 本地变量：生效范围仅为当前shell进程；（其他shell，当前的子sehll进程均无效）
  - 变量赋值：name = “value”
- 环境变量：生效范围为当前shell进程及子进程
  - 变量声明1：export name = “value”
  - 变量声明2：declare -x name = “value”
  - bash中有许多内建的变量环境：SHELL,PATH等等
- 局部变量：生效范围为当前shell进程中某代码片断（通常指函数）
- 位置变量： 2…来表示，让脚本在脚本代码中调用通过命令行传递给它的参数；
- 特殊变量：? 0 * @ #

```bash
$1,$2,…：对应调用第1，第2等参数
$0：命令本身
$*：传递给脚本的所有参数（把所有参数当作整体）
$@：传递给脚本的所有参数
$#：传递给脚本的参数的个数
案例1：
myecho.sh
#!/bin/bash
echo "命令本身是：$0"
echo "第一个参数是：$1"
echo "第二个参数是：$2"
echo "一共有$#个参数"
echo "所有参数是：$@"

案例2：判断所给文件的行数
linecount.sh
#!/bin/bash
linecount="$(wc -l $1|cut -d' ' -f1)"
echo "This file have ${linecount} lines"
```

# 数组

- 语法格式

```bash
语法格式：array_name=(value1 ... valuen)
示例：
my_array=(A B C D)
array_name[0]=value0
array_name[1]=value1
array_name[2]=value2
```

- 读取数组

```bash
读取数组：${array_name[index]}
获取数组中的所有元素：
my_array[0]=A
my_array[1]=B
my_array[2]=C
my_array[3]=D
echo "数组的元素为: ${my_array[*]}"
echo "数组的元素为: ${my_array[@]}"
```

- 获取数组的长度

```bash
获取数组的长度：
my_array[0]=A
my_array[1]=B
my_array[2]=C
my_array[3]=D
echo "数组元素个数为: ${#my_array[*]}"
echo "数组元素个数为: ${#my_array[@]}"
```

# 算数运算

- 运算符

```bash
+ ‐ * / % ** ...
增强赋值：
+=，‐=，*=，/=，%=
乘法符号有些场景中需要转义 : *\
bash有内建随机数生成器：$RANDOM
```

- 完成算数运算

```bash
(1) let var（变量名）=算术表达式
(2) var=$[算术表达式]
(3) var=$((算术表达式))
(4) var=$(expr arg1 arg2 arg3 …)                var=$(expr 1 + 2)
expr本身是一个命令，可以直接进行运算
[root@centos73 ~]# expr 1+2
1+2
[root@centos73 ~]# expr 1 + 2注意要空格才可以进行运算。乘法符号有些场景中需要转义，如*。也就是expr这个命令后面跟的是3个参数
3
```

- 练习题

```bash
练习1：计算/etc/passwd文件中第10个用户的第20个用户的ID之和

练习2：传递两个文件路径参数给脚本，计算这两个文件之中所有空白行之和

练习3：统计/etc/,/var/,/usr/目录下有多少目录和文件
```

# 条件测试

测试命令：test EXPERSSION

```bash
num1=100
num2=100
if test $[num1] -eq $[num2]
then
	echo '两个数相等！'
else
	echo '两个数不相等！'
fi
```

### 数值测试

 ‐gt：是否大于 

‐ge：是否大于等于 

‐eq：是否等于 

‐ne：是否不等于 

‐lt：是否小于 

‐le：是否小于等于

练习题，比较两个数的大小

```bash
[root@localhost ~]# cat diff.sh
#!/bin/bash
read -p "请输入两个整数" num1 num2
if [ $num1 -gt $num2 ];then
  echo "$num1 > $num2"
elif [ $num1 -lt $num2 ];then
  echo "$num1 < $num2"
else
  echo "$num1 = $num2"
fi
```

### 字符串测试

```bash
==：是否等于
>：是否大于
<：是否小于
！=：是否不等于
=~：左侧字符串是否能够被右侧的PATTERN所匹配
Note：此表达式一般用于[[ ]]中
‐z “STRING”:测试字符串是否为空，空则为真，不空则为假
‐n “STRING”:测试字符串是否不空，不空则为真，空则为假
```

### 文件测试

```bash
简单的存在性测试：
‐a FILE ：文件存在性测试，存在为真，否则为假
存在性及类型测试：
‐b FLIE：是否存在且为块设备文件；
‐c FILE：是否存在且为字符设备文件；
‐d FILE：是否存在且为目录文件；
‐f FILE：是否存在且为普通文件；
‐h FILE 或 ‐L FILE : 存在且为符号链接文件；
‐p FIEL ：是否存在且为命名管道文件；
‐S FILE：是否存在且为套接文件；
文件权限测试：
‐r FILE：是否存在且可读
‐w FILE：是否存在且可写
‐x FILE：是否存在可执行
文件特殊权限测试：
‐u FILE：是否存在且拥有suid权限；
‐g FILE：是否存在且拥有sgid权限；
‐k FILE：是否存在且拥有sticky权限；
文件大小测试：
‐s FILE：是否存在且非空
文件是否打开：
‐fd：fd表示文件描述符是否已经打开且与某终端相关
‐N FILE：文件自动上一次读取之后是否被修改过；
‐O FILE：当前用户是否为文件的属主；
‐G FILE：当前有效用户是否为文件数组；
双目测试：
FILE1 ‐ef FILE2 ：FILE1与FILE2是否指向同一个设备上的相同inode
FILE1 ‐nt FILE2：FILE1是否新于FILE2
FILE1 ‐ot FILE2：FILE1是否旧于FILE2
```

### 组合测试条件

```bash
逻辑运算符：
&&代表的意思是当前一个命令执行成功时会继续执行后续的命令，当前一个命令执行失败的时候不会执行后续的命令
||代表的意思是当前一个命令执行成功时不会继续执行后续的命令，当前一个命令执行失败的时候会执行后续的命令
第一种方式：
COMMAND1 && COMMAND2
COMMAND1 || COMMAND2
! COMMAND
第二种方式：
EXPRESSION1 ‐a EXPRESSION2
EXPRESSION1 ‐o EXPRESSION2
! EXPRESSION
Note：必须使用测试命令进行
```

## 选择执行

- 单分支选择结构

```bash
if 判断条件；then
条件为真的分支代码
fi
```

- 双分支选择结构

```bash
if 判断条件；then
条件为真的分支代码
else
条件为假的分支代码
fi
```

- 多分支选择结构

```bash
if 判断条件;then
条件为真的分支代码
elif 判断条件;then
条件为真的分支代码
else
条件为假的分支代码
fi
```

- 练习题

```bash
练习1：判断两个数是否相等

Note：if经常会与test命令一起使用
练习2：判断用户是否存在，如果不存在添加用户，并设置密码和用户相同
```

```bash
# 练习2
#!bin/bash
read -p "请输入用户名：" user
id $user &> /dev/null
thing="$(echo $?)"
# echo $thing
if test $[thing] -eq 0
then 
  echo "用户已存在"
else
  useradd $user
  echo $user | passwd --stdin $user &>/dev/null
  echo -e "账户已id成功创建\n"
  echo "密码已更新为账户名"$user
fi
```

# 用户交互

## read命令

- 常用选项：
  - ‐a：将内容读入到数组中
    - echo ‐n "Input muliple values into an array:"
    - read ‐a array
    - echo "get ${#array[@]} values in array"
  - ‐d ： 表示delimiter，即定界符，例如输入为 hello m，有效值为“hello”，请注意m前面的空格等会被删除
  - ‐e ：只用于互相交互的脚本
  - ‐n ： 用于限定最多可以有多少字符可以作为有效读入
  - ‐p ：用于给出提示符，例如：echo –n “…“来给出提示符，可以使用read –p ‘… my promt?’value的方式只需一个语句来表示
  - ‐r ：特殊字符生效(/n等)，也应采用‐r选项。
  - ‐s ： 对于一些特殊的符号不打印的情况
  - ‐t ：用于表示等待输入的时间(s),等待时间超过，将继续执行后面的脚本

- 练习1：提示为："input your name:"，输入姓名后，进行输出

```bash
#!/bin/bash
read ‐p "input your name:" name
echo ${name}
exit 0
```

- 练习2：读取test.txt文本，输出格式为: linecount:context

```bash
test.txt
aaa
bbb
ccc
```

# 循环语句

## for循环

- 循环体：需要执行的语句，可能执行n遍
- 语法

```bash
for 变量名 in 列表;do
循环体
done
```

- 执行机制：依次将列表中的元素赋值给“变量名”；每次赋值后执行一次循环体；直到列表中的元素 耗尽，循环结束
- 练习题1：创建用户user1‐user10家目录，并且在user1‐10家目录下创建1.txt‐10.txt

```bash
#!/bin/bash
for i in {1..10};do
mkdir /home/user$i
for i in (seq 1 10);do
touch /home/user$i/user$i.txt
done
done
练习题2：列出/var/目录下各个子目录占用磁盘大小
```

- 练习题2：批量测试地址是否在线

```bash
[root@localhost ~]# cat ping.sh
#!/bin/bash
for address in {1..255}
do 
  #echo $address
  ping -c1 -W1 192.168.111.$address &>/dev/null
  status="$(echo $?)"
  if test $status -eq 0
  then
    echo "192.168.111.$address 目前在线"
  else
    echo "192.168.111.$address 目前不在线"
  fi
done
```

## while循环

- 语法

```bash
while 测试条件;do
循环体
done
```

- 经典使用

```bash
#!/bin/bash
while read a;do
echo $a
done < /datas/6files
```

- 练习题1：计算1+2+..10

```bash
#!/bin/bash
```

- 练习题2，猜随机数游戏

```bash
[root@localhost ~]# cat ran.sh
#!/bin/bash
num="$(echo $[RANDOM%100+1])"
count=0
echo $num
while true
do
read -p "请猜一下这个数字：" guess
let count=${count}+1
echo $guess
  if [ $guess -lt $num ];then
    echo "你猜的小了，再猜一次吧"
  elif [ $guess -gt $num ];then
    echo "你猜的大了，再猜一次吧"
  else
    echo "你猜对了 总共猜了$count次"
    breakh*+
done
```

## until循环

- while的是条件是测真值，until的条件式测假值
- 语法

```bash
until 条件测试;do
循环体
done
```

- 练习1：99乘法表

```bash
#while 写法  当判断条件为真则运行下面的内容
#!/bin/bash
first=1
second=1
while ((first <= 9))
do
  while ((second <= first))
  do
    let chicken=${first}*${second}
    echo -n "${first}*${second}=${chicken} "
    let second++
    done
  let first++
  second=1
  echo ""
  done 
```

```bash
#until 写法  当判断条件为假则运行下面的内容
#!/bin/bash
first=1
second=1
until ((first > 9))
do
  until ((second > first))
  do
    let chicken=${first}*${second}
    echo -n "${first}*${second}=${chicken} "
    let second++
    done
  let first++
  second=1
  echo ""
  done
```

# 函数

- 语法

```bash
function FUNNAME(){
函数体
返回值
}
FUNNME #调用函数
```

- 实例1

```bash
#!/bin/bash
demoFun(){
echo '这是我的第一个 shell 函数!'
}
echo "‐‐‐‐‐函数开始执行‐‐‐‐‐"
demoFun
echo "‐‐‐‐‐函数执行完毕‐‐‐‐‐"
```

- 带返回值并且调用返回值

```bash
funWithReturn(){
echo "这个函数会对输入的两个数字进行相加运算..."
echo "输入第一个数字: "
read aNum
echo "输入第二个数字: "
read anotherNum
echo "两个数字分别为 $aNum 和 $anotherNum !"
return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
#可以使用$?来获取返回值
```

- 函数参数

```bash
funWithParam(){
echo "第一个参数为 $1 !"
echo "第二个参数为 $2 !"
echo "第十个参数为 $10 !"
echo "第十个参数为 ${10} !"
echo "第十一个参数为 ${11} !"
echo "参数总数有 $# 个!"
echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
注意，$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数
```

# 调试脚本

- -x : 在执行时显示参数和命令；
- +x：禁止调试
- -v：当命令行进行读取时显示输入；
- +v：禁止打印输入。
- \- n：检测脚本中的语法错误

![](image-20220813131931161.png)

![](image-20220813131944616.png)

![](image-20220813131953604.png)

![](image-20220813132002639.png)

# 环境配置

## bash配置文件

- 生效范围分类
  - 全局配置：
    - /etc/bashrc
    - /etc/profile
    - /etc/profile.d/*.sh
  - 个人配置
    - ~/.bash_profile
    - ~/.bashrc
- 功能分类
  - profile类：为交互式的shell提供配置
  - bashrc类：为非交换式的shell提供配置
- shell登录
  - 交互式登录：su - USERNAME
  - /etc/profile –> /etc/profile.d/*.sh –> ~/.bash_profile –> ~/.bashrc –> /etc/bashrc
  - 非交换式登录：su USERNAME
- 编辑配置文件定义的新设置的生效方式
  - 重新启动shell进程
  - 使用source命令

## 案例，开机显示系统信息脚本

```bash
#!/bin/bash

function disk_used(){

#获取磁盘使用率脚本
#2022/7/14
 
time=$(date "+%Y-%m-%d %H:%M:%S")
diskUsage=`df -h | sed -n '6p' | awk '{print $5}'`
disk=`echo "$diskUsage" | cut -d "%" -f 1`
disk_used=`df -h | sed -n '6p' | awk '{print $3}'`
disk_left=`df -h | sed -n '6p' | awk '{print $4}'`
disk_total=`df -h | sed -n '6p' | awk '{print $2}'`
echo "磁盘总量：$disk_total 磁盘使用量：$disk_used 磁盘剩余量：$disk_left"
echo "磁盘使用率：$disk%"
if [ $disk -gt 70 ]   #此处10可更改大小
then
     echo "警告，您当前磁盘使用率${disk}%，已严重超标$time 标准最高使用量为70%" 
else
    exit
fi
}

function RAM_used(){

 
#获取内存相关信息的脚本
 
#2022/7/14
 
time=$(date "+%Y-%m-%d %H:%M:%S")
memoryUsed=`free -m | sed -n '2p' | awk '{printf "%f\n",($3)/$2*100}'`
memoryleft=`free -h | sed -n '2p' | awk '{print $4}'`
memorytotal=`free -h | sed -n '2p' | awk '{print $2}'`
echo "内存总量为$memorytotal 内存剩余量为$memoryleft"
echo "内存使用量:${memoryUsed}% ${time}"
memory=`echo "$memoryUsed" | cut -d "." -f 1`
if [ $memory -gt 70 ]
then
    echo "警告，您当前内存使用率${memoryUsed}%，已严重超标$time 标准最高使用率为70%"
else
    return
fi
}

function CPU_used(){
#
#脚本功能描述：依据/proc/stat文件获取并计算CPU使用率
#
#CPU时间计算公式：CPU_TIME=user+system+nice+idle+iowait+irq+softirq
#CPU使用率计算公式：cpu_usage=(idle2-idle1)/(cpu2-cpu1)*100
#默认时间间隔
TIME_INTERVAL=5
time=$(date "+%Y-%m-%d %H:%M:%S")
LAST_CPU_INFO=$(cat /proc/stat | grep -w cpu | awk '{print $2,$3,$4,$5,$6,$7,$8}')
LAST_SYS_IDLE=$(echo $LAST_CPU_INFO | awk '{print $4}')
LAST_TOTAL_CPU_T=$(echo $LAST_CPU_INFO | awk '{print $1+$2+$3+$4+$5+$6+$7}')
sleep ${TIME_INTERVAL}
NEXT_CPU_INFO=$(cat /proc/stat | grep -w cpu | awk '{print $2,$3,$4,$5,$6,$7,$8}')
NEXT_SYS_IDLE=$(echo $NEXT_CPU_INFO | awk '{print $4}')
NEXT_TOTAL_CPU_T=$(echo $NEXT_CPU_INFO | awk '{print $1+$2+$3+$4+$5+$6+$7}')
 
#系统空闲时间
SYSTEM_IDLE=`echo ${NEXT_SYS_IDLE} ${LAST_SYS_IDLE} | awk '{print $1-$2}'`
#CPU总时间
TOTAL_TIME=`echo ${NEXT_TOTAL_CPU_T} ${LAST_TOTAL_CPU_T} | awk '{print $1-$2}'`
CPU_USAGE=`echo ${SYSTEM_IDLE} ${TOTAL_TIME} | awk '{printf "%.2f", 100-$1/$2*100}'`
echo "CPU使用率:${CPU_USAGE}% "$time
 
cpu=`echo "$CPU_USAGE" | cut -d "." -f 1`
if [ $cpu -gt 80 ]
then
    echo "警告，您当前CPU使用率${CPU_USAGE}%，已严重超标"$time 
else
    return
fi
}

yum install -y net-tools &> /dev/null
wangka=`ip a | grep ens | head -1 | cut -d: -f2`
System=$(hostnamectl | grep System | awk '{print $3,$4,$5}')
Kernel=$(hostnamectl | grep Kernel | awk -F: '{print $2}')
Virtualization=$(hostnamectl | grep Virtualization| awk '{print $2}')
Statichostname=$(hostnamectl | grep Static|awk -F: '{print $2}')
Ens32=$(ifconfig $wangka | awk 'NR==2 {print $2}')
Lo=$(ifconfig lo0 | awk 'NR==2 {print $2}')
NetworkIp=$(curl -s icanhazip.com)
CPU_name=$(lscpu | grep 名称| awk -F"  +" '{print $2}')
CPU_core=$(lscpu | grep CPU\(s\): | awk -F"  +" '{print $2}')
echo "当前系统版本是：$System"
echo "当前系统内核是：$Kernel"
echo "当前虚拟平台是：$Virtualization"
echo "当前主机名是：$Statichostname"
echo "当前网卡$wangka的地址是：$Ens32"
echo "当前lo0接口的地址是：$Lo"
echo "当前公网地址是：$NetworkIp"
echo "当前使用的CPU型号为：$CPU_name"
echo "当前CPU核数为：$CPU_core"
CPU_used
RAM_used
disk_used
```

## 案例，监控mysql进程

> 需求：
> 1.每隔10s监控mysql的进程数，若进程数大于等于500，则自动重启mysqld服务，并检测服务是 否重启成功
> 2.若未成功则需要再次启动，若重启5次依旧没有成功，则向管理员发送告警邮件（使用echo输 出已发送即可），并退出检测
> 3.如果启动成功，则等待1分钟后再次检测mysql进程数，若进程数正常，则恢复正常检测（10s 一次），否则放弃重启并向管理员发送告警邮件，并退出检测

```shell
# 代码可能不太对
#!/bin/bash
function check_mysqld_process_number() {
process_num=`ps -ef | grep mysql| wc -l`

if [ $process_num -gt 50 ];then
    systemctl restart mysqld &> /dev/null
    # 重启五次mysqld确保服务启动
    systemctl status mysqld &> /dev/null
    num_restart_httpd=0 #定义一个变量进行重启次数的记录
    if [ $? -ne 0 ];then #如果重启后mysqld运行状态不正常
        while true;do
            let num_restart_mysqld++ #记录重启次数来确定最高循环五次
            systemctl restart httpd &> /dev/null
            systemctl status httpd &> /dev/null
            [ $? -eq 0 ] && break #如果有一次成功启动则不在重新执行重启
            [ $num_restart_httpd -eq 6 ] && break #重启次数达到6次不再尝试重启解决
        done
    fi

    # 判断重启服务的结果
    systemctl status httpd &> /dev/null
    [ $? -ne 0 ] && echo "apache未正常重启，已发送邮件给管理员" && return 1 #发现重启无法解决 通知管理员
    sleep 60
    return 0
    # 再次判断进程是否正常
    process_num=`ps -ef | grep httpd| wc -l`
    if [ $process_num -gt 50 ] ;then
        echo "apache经过重启进程数依然大于50"
        return 1
    else
        return 0
    fi
else
    echo "进程数小于50"
    sleep 10 #10秒进行一次检测
    return 0
fi
}
# 每十秒钟执行一次函数，检查进程是否正常
while true;do
    check_mysqld_process_number
    [ $? -eq 1 ] && exit
done
```

# 代码练习：

## 检查两个目录下的所有文件是否有相同的，输出相同文件以及所有各自有的的文件

```shell
#!/bin/bash

read -p "请输入第一个文件夹的路径：" DIR1
read -p "请输入第二个文件夹的路径：" DIR2
#echo $DIR1
#echo $DIR2

#用于读取 DIR1 中文件的数目
if [ ! -d $DIR1 ];then # 判断用户输入的路径是否正确，错误则直接退出程序
    echo "没有 $DIR1 这个目录，程序退出"
    exit
fi
count_1=`find $DIR1 |wc -l`
let count_1=${count_1}-1 #find命令第一行实际是查找的目录，减去第一行才是是实际的文件数
#echo $count_1
#echo $DIR1

#用于读取 DIR1 中文件的数目
if [ ! -d $DIR2 ];then # 判断用户输入的路径是否正确，错误则直接退出程序
    echo "没有 $DIR2 这个目录，程序退出"
    exit
fi
count_2=`find $DIR2 |wc -l`
let count_2=${count_2}-1 #find命令第一行实际是查找的目录，减去第一行才是是实际的文件数
#echo $count_2
#echo $DIR2

#将DIR1中的文件以“文件名 md5值”形式放入temp_1
time=0
temp=2
while [ $time -lt $count_1 ];do #利用DIR1中的文件数目控制循环
    file_locate_1=`find $DIR1 | sed -n "${temp}p"` #取出文件名
    #echo $temp
    #echo $file_locate_1
    file_first=`md5sum $file_locate_1 | awk -F[' ']+ '{print $1}'` #取出文件对应的md5值
    #echo $file_first
    echo "$file_locate_1 $file_first" >> /root/temp_1 #格式化输入文件
    let time=${time}+1
    let temp=${temp}+1
done

#将DIR2中的文件以“文件名 md5值”形式放入temp_2
time=0
temp=2
while [ $time -lt $count_2 ];do #利用DIR2中的文件数目控制循环
    file_locate_2=`find $DIR2 | sed -n "${temp}p"`  #取出文件名
    #echo $temp
    #echo $file_locate_2
    file_second=`md5sum $file_locate_2 | awk -F[' ']+ '{print $1}'`  #取出文件对应的md5值
    #echo $file_second
    echo "$file_locate_2 $file_second" >> /root/temp_2 #格式化输入文件
    let time=${time}+1
    let temp=${temp}+1
done

out=0
in=0
hang=1
hang_2=1
while [ $out -lt $count_1 ];do #外层循环由DIR1中的文件数目控制
    file_name=`cat /root/temp_1 | sed -n "${hang}p" | awk -F" " '{print $1}'`
    file_md5=`cat /root/temp_1 | sed -n "${hang}p" | awk -F" " '{print $2}'`
    while [ $in -lt $count_2 ];do #内层循环由DIR2中的文件数目控制
        file_name_2=`cat /root/temp_2 | sed -n "${hang_2}p" | awk -F" " '{print $1}'`
        file_md5_2=`cat /root/temp_2 | sed -n "${hang_2}p" | awk -F" " '{print $2}'`
        if [ "${file_md5}" = "${file_md5_2}" ];then #将文件的md5值进行对比
            echo "$DIR1 中的 $file_name 与 $DIR2 中的 $file_name_2 为相同文件" #相同的则输出
            flag=1
            echo "$file_name 1" >> /root/temp_3 #第一个文件夹的放入temp_3
            echo "$file_name_2 1" >> /root/temp_4 #第二个文件夹的放入temp_4
        else #[ $flag -eq 0 ];then #该组对比表示没有对比上
            echo "$file_name 0" >> /root/temp_3 #第一个文件夹的放入temp_3
            echo "$file_name_2 0" >> /root/temp_4 #第二个文件夹的放入temp_4
        fi
        let in=${in}+1
        let hang_2=${hang_2}+1
        flag=0
    done
    hang_2=1
    in=0
    #echo $out
    let out=${out}+1
    let hang=${hang}+1
    # echo $file_name
    #echo $out
    #echo $count_1
done

if [ -f /root/temp_3 ];then
    echo "这是 $DIR1 中特有的文件"
    sort -n /root/temp_3 | uniq | awk -F" " '{print $1}' | uniq -u #去除重复后取第一个元素后再次去掉重复行
    # echo "这是 $DIR2 中特有的文件"
    # sort -n /root/temp_4 | uniq
else
    echo "$DIR1 中的每个文件都与 $DIR2 中相同"
fi
if [ -f /root/temp_4 ];then
    echo "这是 $DIR2 中特有的文件"
    sort -n /root/temp_4 | uniq | awk -F" " '{print $1}' | uniq -u  #去除重复后取第一个元素后再次去掉重复行
else
    echo "$DIR2 中的每个文件都与 $DIR1 中相同"
fi
rm -f /root/temp_{1..4}
```
