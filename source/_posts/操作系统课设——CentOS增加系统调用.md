---
title: 操作系统课设——CentOS增加系统调用
tags: [课程设计]
categories: [操作系统]
index_img: /Photo/Blog_Photo/index/操作系统课设——CentOS增加系统调用.jpg
banner_img: /Photo/Blog_Photo/Banner/操作系统课设——CentOS增加系统调用.jpg
typora-root-url: 操作系统课设——CentOS增加系统调用
date: 2023-06-12 22:24:07
---

# 操作系统课设——CentOS增加系统调用

## 一、具体任务

采用编译内核法，在Linux中增加一个系统调用。

 要求： 

1. 系统调用实现的功能：计算一个数字的三次方， 并打印出来。 
2. 另外写一个程序进行调用

相关思路：

本次实验实在CentOS 7系统中对于Linux内核源码进行修改，并对源码进行编译，最后完成切换内核操作，并在C语言程序中进行系统调用。

具体步骤：

- 下载Linux 4.20.4版本的源码
- 安装所需的工具和相关的编译环境
- 对源码进行修改并增加功能
- 对原本的系统环境内进行系统调用的添加
- 进行Linux内核的编译
- 编写C语言程序并在其中对添加的功能进行验证

## 二、CentOS系统的安装

本次系统安装采用了CentOS 7的系统，在VMware WorkStation中安装具体过程不在赘述，注意，尽量将CPU核数给多一些，以免编译的时间过长,建议存储空间大于40GB，防止出现内存不足的情况。

**注意：安装完成以及下面每一步进行记得一定要打上快照，否则出现错误重新操作异常困难，养成打快照的好习惯**

![CentOS安装完成示例](image-20230613081816078.png)

## 三、安装相关的系统环境

使用yum包管理工具将编译需要的相关工具进行安装，为下一步编译进行准备

```bash
sudo yum -y install ncurses-devel
sudo yum -y install bc
sudo yum -y install bison
sudo yum -y install flex
sudo yum -y install gcc g++ gdb make
sudo yum -y install centos-release-scl
sudo yum -y install devtoolset-7-gcc*
sudo yum -y install elfutils-libelf-devel
sudo yum -y install openssl-devel
sudo yum -y install zlib zlib-devel pcre pcre-devel gcc gcc-c++ openssl openssl-devel libevent libevent-devel perl unzip net-tools wget
```

## 四、对源码进行下载并修改

### 下载源码

Linux的源码可以从其官方的网站下载，这里选择了Linux-4.20.4版本的源码

```bash
# 下载速度因人而异，我的网络环境比较好大概两三秒就下载完成了，也有人出现需要下载一两个小时的
# 建议采取一些措施改善一下网络环境
sudo wget http://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.20.4.tar.xz
```

![源码下载完成](image-20230613094521121.png)

### 修改源码

刚才我们将源码下载到了我们当前用户的根目录下，一般的CentOS 7使用的应该是root用户进行登录的，那么此时你的下载的源包就在/root目录下。

修改/usr/src/kernels/linux-4.20.4/kernel/sys.c文件

![文件末尾添加自定义函数](image-20230613094617052.png)

```c
// 文件最后加入我们自己的功能调用
SYSCALL_DEFINE1(cube,int,num){
    int result = num*num*num;
    return result;
}
```

修改/usr/src/kernels/linux-4.20.4/arch/x86/include/asm/syscalls.h文件

![添加声明](image-20230613094824243.png)

```c
// 在这个文件内我们可以添加我们自己的声明
// 在/*kernel/ioport.c*/下下面进行添加
asmlinkage long sys_cube(long n);
```

## 五、添加系统调用号

修改/usr/src/kernels/linux-4.20.4/arch/x86/entry/syscalls/syscall_64.tbl文件

![添加335号系统调用](image-20230613094906717.png)

```
# 第一个是系统调用号
335  64  cube  __x64_sys_cube
```

## 六、准备并进行编译

```bash
cp /boot/config-3.10.0-1160.71.1.el7.x86_64 /usr/src/kernels/linux-4.20.4/.config
make menuconfig # 对Config进行再设置
```

这里注意，在make设置的时候，移动光标直接选择load，由于Linux会隐藏点开头的文件，.config以及刚刚我们复制过来的文件config文件只有`ll`命令才能看见，`ls`命令无法查看。

![make menuconfig](image-20230613095404563.png)

选择load之后即可选择yes即可配置完成，将当前界面退出，使用`ll -a`命令我们可以发现原本的`.config`文件已经变成了`.config.old`,并且生成了一个新的`.config`文件，如下图。

![文件已经发生了变化](image-20230613215136957.png)

下面我们可以进行编译了

```bash
# 进行编译 时间可能比较长 16指的是使用多少核心去执行编译 我自己的电脑分配了16核 大概需要20分钟 根据个人电脑动态调整核数
make -j16
# 安装模块和内核
make modules_install
make install
# 更新引导文件
grub2-mkconfig -o /boot/grub2/grub.cfg
```

## 七、写程序进行验证

### 编写验证程序

新建一个C语言程序test.c进行验证

```c
// 输入以下代码
#include<stdio.h>
#include<linux/kernel.h>
#include<sys/syscall.h>
#include<unistd.h>
int main(){
        double n;
        long s;
        printf("请输入一个数字：");
        scanf("%lf",&n);
        s=n*100;
        if(s<0){
                s = -syscall(335,-s);
        }
        else{
                s = syscall(335,s);
        }
        n = s;
        n = n / syscall(335,100);
        printf("结果为：");
        printf("%lf",n);
        return 0;
}
```

### 测试结果

![验证结果](image-20230613102024617.png)
