---
title: Python操作文件的基本流程
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python操作文件的基本流程.jpg
banner_img: /Photo/Blog_Photo/Banner/Python操作文件的基本流程.jpg
typora-root-url: Python操作文件的基本流程
date: 2022-09-05 12:33
---

# 操作文件的基本流程

## 操作文件的函数

| 序号 | 函数/方法 | 说明                           |
| ---- | --------- | ------------------------------ |
| 01   | open      | 打开文件，并且返回文件操作对象 |
| 02   | read      | 将文件内容读取到内存           |
| 03   | write     | 将指定内容写入文件             |
| 04   | close     | 关闭文件                       |

- open  函数在把文件打开的同时返回文件对象
- 其余的三个函数都需要对文件对象进行操作才能够有效运用

### open函数

1. 第一个参数是文件名（文件名区分大小写）第二个参数是打开方式； 
2. 如果文件存在返回文件操作对象； 
3. 如果文件不存在抛出异常

### read函数

可以一次性读入并返回文件的所有内容

### close函数

关闭文件

### 演示代码

```python
# 1. 打开 - 文件名需要注意大小写
file = open("README")

# 2. 读取
text = file.read()
print(text)

# 3. 关闭
file.close()

#打开后进行操作
file = open("README","w")
file.write("hello everyone")
file.close()
```

## 文件打开方式

```python
f = open("文件名", "访问方式")
```

### 访问方式说明

| 访问方式 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| r        | 以只读方式打开文件。文件的指针将会放在文件的开头，这是默认模式。如果文件不存 在，抛出异常 |
| w        | 以只写方式打开文件。如果文件存在会被覆盖。如果文件不存在，创建新文件 |
| a        | 以追加方式打开文件。如果该文件已存在，文件指针将会放在文件的结尾。如果文件不 存在，创建新文件进行写入 |
| r+       | 以读写方式打开文件。文件的指针将会放在文件的开头。如果文件不存在，抛出异常 |
| w+       | 以读写方式打开文件。如果文件存在会被覆盖。如果文件不存在，创建新文件 |
| a+       | 以读写方式打开文件。如果该文件已存在，文件指针将会放在文件的结尾。如果文件不 存在，创建新文件进行写入 |

以bytes类型操作的读写，写读模式(这种方式是仅对非文本文件)

| r+b  | 读写【可读，可写】 |
| ---- | ------------------ |
| w+b  | 写读【可写，可读】 |
| a+b  | 写读【可写，可读】 |

对于非文本文件，我们只能使用b模式，"b"表示以字节的方式操作（而所有文件也都是以字节的形式存 储的，使用这种模式无需考虑文本文件的字符编码、图片文件的jgp格式、视频文件的avi格式）

在函数中写的时候不需要加上+号，需要直接写  rb  wb  ab

### 按行读取文件内容

- read方法默认会把文件的所有内容一次性读取到内存
- readline方法可以一次读取一行内容；
- 方法执行后，文件指针移动到下一行，准备再次读取；

### with结构

with结构的好处是不需要进行再次手动去关闭文件

```python
with open("README.txt") as file1:#with结构这样使用，获取的文件对象就是file1
	while True:
		text1 = file1.readline().strip()
		if text1:
			print("这是第%s行内容" % i)
			i += 1
			print(text1)
		else:
			break
            
#将一个文件的内容放到另外一个文件内
with open('a.txt','r') as read_f,open('b.txt','w') as write_f:
	data=read_f.read()
	write_f.write(data)
```

### 文件编码

f=open(...)是由操作系统打开文件，那么如果我们没有为open指定编码，那么打开文件的默认编码很明 显是操作系统说了算了，操作系统会用自己的默认编码去打开文件，在windows下是gbk，在linux下是 utf-8。

通过这个方式可以将文件强制使用某种编码方式去读取，而非使用操作系统本身默认的编码方式

```python
f=open('a.txt','r',encoding='utf-8'
```

## 文件的操作方法

### 常用操作方式

read（3）：

1. 文件打开方式为文本模式时，代表读取3个字符
2. 文件打开方式为b模式时，代表读取3个字节

文本文件所有的操作方式整理：

```python
def close(self, *args, **kwargs): # real signature unknown
		关闭文件
		pass
def fileno(self, *args, **kwargs): # real signature unknown
		文件描述符
		pass
def flush(self, *args, **kwargs): # real signature unknown
		刷新文件内部缓冲区
		pass
def isatty(self, *args, **kwargs): # real signature unknown
		判断文件是否是同意tty设备
		pass
def read(self, *args, **kwargs): # real signature unknown
		读取指定字节数据
		pass
def readable(self, *args, **kwargs): # real signature unknown
		是否可读
		pass
def readline(self, *args, **kwargs): # real signature unknown
		仅读取一行数据
		pass
def seek(self, *args, **kwargs): # real signature unknown
		指定文件中指针位置
		pass
def seekable(self, *args, **kwargs): # real signature unknown
		指针是否可操作
		pass
def tell(self, *args, **kwargs): # real signature unknown
		获取指针位置
		pass
def truncate(self, *args, **kwargs): # real signature unknown
		截断数据，仅保留指定之前数据
		pass
def writable(self, *args, **kwargs): # real signature unknown
		是否可写
		pass
def write(self, *args, **kwargs): # real signature unknown
		写内容
		pass
def __getstate__(self, *args, **kwargs): # real signature unknown
		pass
def __init__(self, *args, **kwargs): # real signature unknown
		pass
@staticmethod # known case of __new__
def __new__(*args, **kwargs): # real signature unknown
	""" Create and return a new object. See help(type) for accurate
signature. """
	pass
def __next__(self, *args, **kwargs): # real signature unknown
	""" Implement next(self). """
	pass
def __repr__(self, *args, **kwargs): # real signature unknown
	""" Return repr(self). """
	pass
```

## 案例一、文件的修改

文件的数据是存放于硬盘上的，因而只存在覆盖、不存在修改这么一说，我们平时看到的修改文件，都 是模拟出来的效果，具体的说有两种实现方式：

 方式一：将硬盘存放的该文件的内容全部加载到内存，在内存中是可以修改的，修改完毕后，再由内存 覆盖到硬盘（word，vim，nodpad++等编辑器）

```python
import os#与操作系统的指令相连接

with open('a.txt') as read_f,open('a.txt.new','w') as write_f:
	data = read_f.read()
	data = data.replace('Hello','nihao')
	
	write_f.write(data)
os.remove('a.txt')#让操作系统删除a.txt文件
os.rename('a.txt.new','a.txt')#让操作系统重命名a.txt.new为a.txt
```

方式二：将硬盘存放的该文件的内容一行一行地读入内存，修改完毕就写入新文件，最后用新文件覆盖源文件

```python
import os

with open('a.txt') as read_f,open('a.txt.new','w') as write_f:
	for line in read_f:
		line = line.replace('nihao','Hello')
		write_f.write(line)

os.remove('a.txt')
os.rename('a.txt.new','a.txt')
```

## 案列二、完成文件的复制

```python
file = open("README")#打开文件
while True:
	text = file.readline()
	print(text)#直接对读取到的这一行进行打印
	if not text:#如果发现文件读取到已经读取不到文件时退出
		break
file.close()
# --------------------------------------------------------------------------
-----------
# 小文件复制
file1 = open("README", "r")#以阅读的方式打开需要复制的文件
file2 = open("README[复件]", "w")#以写入的方式打开要被复制到的文件

text = file1.read()#将文件1内的内容一次性宣布读取出来
file2.write(text)#将文件1内读取的内容一次性全部写入文件2

file1.close()#将文件1与文件2全部关闭
file2.close()

# --------------------------------------------------------------------------
-----------
# 大文件复制
#由于文件太大，如果像上一个方法一样将文字全部吃入内存，会对内存造成较大的负担
file3 = open("README", "r")
file4 = open("README[大文件复制]", "w")

while True:#通过循环的建立
	text = file3.readline()#将一行读取出来
	if not text:#如果这一行读取不到东西了，接直接退出
		break
	file4.write(text)#否则将读到的东西写入文件
	
file3.close()
file4.close()
```

## 案例三、计算总价

文件a.txt内容：每一行内容分别为商品名字，价钱，个数。

```
apple 10 3
tesla 100000 1
mac 3000 2
lenovo 30000 3
chicken 10 3
```

通过代码，将其构建成这种数据类型：[{'name':'apple','price':10,'amount':3}, {'name':'tesla','price':1000000,'amount':1}......] 并计算出总价钱。

自己的代码：

```python
i = 0
num = 0
with open("price.txt", "r") as file:
    while True:
        text = file.readline().strip()#将从文件中读取的行去掉空的字符
        if text:
            ret = text.split(" ")#通过分割函数，将这个一行分割成几个不同的元素存放在列表中
            information = {"name": ret[0], "price": ret[1], "amount": ret[2]}#通过索引将这些信息制作成字典，放进information
            unit_price = int(information["price"]) * int(information["amount"])#计算一个商品的总价格
            print(unit_price)
            num = num + unit_price#借助循环将所有的价格相加
            list = []#为满足题目要求强行制作列表
            list.append(information)将每次循环产生的新信息放入列表
            print(list)
        else:
            print(num)
            break
```

## 案例四、注册登录

将之前写的注册登录完善，采用文件进行记录

```python
def read():
    information = {}
    with open('station.txt', 'r') as file:  # 打开外部存放数据的文件
        while True:
            text = file.readline().strip()
            if text:  # 将拿出来的东西进行切片
                ret = text.split(':')
                # print(ret)
                information[ret[0]] = ret[1]  # 存放在字典里面
                # print (list)
            else:
                return information


def log(dic):
    while True:
        new_account = input("请输入新的账号： ")
        new_password = input("请输入新的密码： ")
        print("正在检测账号密码是否符合系统要求")
        # 首先验证其大小，采用与一个八位的字符串进行大小比较
        # 其次对于其重要包含字母和数字的比较方法可以采用集合的交集，如果分别与字母集合和数字集合比较都出现交集，则满足条件
        # 这个函数可以直接判断是否全部为数字或者全部为字母
        if new_password < "12345678" or new_password.isalpha() == True or new_password.isdigit() == True:

            print("密码不符合要求，不能是纯数字或纯字母，重新输入密码")
            continue
        else:  # 将相应的符合要求的账号密码存入字典
            dic[new_account] = new_password
            with open('station.txt', 'a') as file:
                amazing = "\n%s:%s" % (new_account, new_password)
                file.write(amazing)
            # information_station.setdefault(new_account, new_password)我觉得孙茂哥的表达方式更为贴近于英文，表达性更强
            print("账号符合要求，注册成功")  # 鉴于Python边编译边运行的特性，写入成功后再进行打印，防止写入报错
            break


def visit(dic):
    while True:
        account = input("请输入您的账号： ")
        password = input("请输入您的密码： ")
        if account in dic:  # 首先有个判断语句说明if account in information_station可以解决报错问题
            if password == dic[account]:
                print("欢迎进入系统")
                while True:
                    Grade = input("请输入你的成绩\n")  # 如果输入的不是数，会发生报错，该如何解决这个问题？
                    # 可以通过先判断是不是输入了数字，是数字再强行转换，不是数字直接提示输入错误
                    if Grade.isalpha() == True:
                        Grade = int(Grade)
                        if Grade < 60:
                            print("你的成绩不及格")
                        elif Grade >= 60 and Grade < 70:
                            print("一般般勉强及格")
                        elif Grade >= 70 and Grade < 80:
                            print("良好")
                        elif Grade >= 80 and Grade < 90:
                            print("不错")
                        elif Grade >= 90 and Grade <= 100:
                            print("很优秀")
                        else:
                            print("你输入的不是正确的分数")
                    else:
                        print("输入格式错误")
            else:
                print("账号或密码错误，请确认后重新输入")
                continue
        else:
            print("没有此账号，请重新输入")


def manage(a, dic={}):
    if a == '1':
        log(dic)
    elif a == '2':
        visit(dic)
    else:
        print("请输入正确的选项数字,程序结束")


def main():
    dic = read()
    while True:
        command = input("输入1进行注册        输入2进行登录        输入3退出系统\n")
        manage(command, dic)


# --------------------------------------------------主函数---------------------------------------------------------------
if __name__ == '__main__':
    main()

```
