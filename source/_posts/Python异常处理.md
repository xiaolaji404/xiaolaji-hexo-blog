---
title: Python异常处理
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python异常处理.jpg
banner_img: /Photo/Blog_Photo/Banner/Python异常处理.jpg
typora-root-url: Python异常处理
date: 2022-10-01 00:03
---

# 异常和错误

## 程序错误

1. 语法错误（这种错误，根本过不了python解释器的语法检测，必须在程序执行前就改正）

```python
#语法错误示范一
if

#语法错误示范二
def test:
pass

#语法错误示范三
print(haha
```

2. 逻辑错误

```python
#用户输入不完整(比如输入为空)或者输入非法(输入不是数字)
num=input(">>: ")
res1 = int(num)

#无法完成计算
res1=1/0
res2=1+'str'
```

## 异常

异常就是程序运行时发生错误的信号 

异常之后的代码就不执行

![报错截图](image-20220708203928952.png)

## 异常种类

在python中不同的异常可以用不同的类型（python中统一了类与类型，类型即类）去标识，不同的类对 象标识不同的异常，一个异常标识一种错误

```
# 触发IndexError
l=['eagle','aa']
l[3]
# 触发KeyError
dic={'name':'eagle'}
dic['age']

#触发ValueError
s='hello'
int(s)
```

常见异常

| AttributeError    | 试图访问一个对象没有的属性，比如foo.x，但是foo没有属性x      |
| ----------------- | ------------------------------------------------------------ |
| IOError           | 输入/输出异常；基本上是无法打开文件                          |
| ImportError       | 无法引入模块或包；基本上是路径问题或名称错误                 |
| IndentationError  | 语法错误（的子类） ；代码没有正确对齐                        |
| IndexError        | 下标索引超出序列边界，比如当x只有三个元素，却试图访问x[5]    |
| KeyError          | 试图访问字典里不存在的键                                     |
| KeyboardInterrupt | Ctrl+C被按下                                                 |
| NameError         | 使用一个还未被赋予对象的变量                                 |
| SyntaxError       | Python代码非法，代码不能编译(个人认为这是语法错误，写错了）  |
| TypeError         | 传入对象类型与要求的不符合                                   |
| UnboundLocalError | 试图访问一个还未被设置的局部变量，基本上是由于另有一个同名的全局 变量，导致你以为正在访问它 |
| ValueError        | 传入一个调用者不期望的值，即使值的类型是正确的               |

其他错误

```
ArithmeticError
AssertionError
AttributeError
BaseException
BufferError
BytesWarning
DeprecationWarning
EnvironmentError
EOFError
Exception
FloatingPointError
FutureWarning
GeneratorExit
ImportError
ImportWarning
IndentationError
IndexError
IOError
KeyboardInterrupt
KeyError
LookupError
MemoryError
NameError
NotImplementedError
OSError
OverflowError
PendingDeprecationWarning
ReferenceError
RuntimeError
RuntimeWarning
StandardError
StopIteration
SyntaxError
SyntaxWarning
SystemError
SystemExit
TabError
TypeError
UnboundLocalError
UnicodeDecodeError
UnicodeEncodeError
UnicodeError
UnicodeTranslateError
UnicodeWarning
UserWarning
ValueError
Warning
ZeroDivisionError
```

# 异常处理

- python解释器检测到错误，触发异常（也允许程序员自己触发异常）
- 程序员编写特定的代码，专门用来捕捉这个异常（这段代码与程序逻辑无关，与异常处理有关）
- 如果捕捉成功则进入另外一个处理分支，执行你为其定制的逻辑，使程序不会崩溃，这就是异常处理

首先须知，异常是由程序的错误引起的，语法上的错误跟异常处理无关，必须在程序运行前就修正

```python
num1=input('>>: ') #输入一个字符串试试
if num1.isdigit():
	int(num1) #我们的正统程序放到了这里,其余的都属于异常处理范畴
elif num1.isspace():
	print('输入的是空格,就执行我这里的逻辑')
elif len(num1) == 0:
	print('输入的是空,就执行我这里的逻辑')
else:
	print('其他情情况,执行我这里的逻辑')
	
'''
问题一：
使用if的方式我们只为第一段代码加上了异常处理，但这些if，跟你的代码逻辑并无关系，这样你的代
码会因为可读性差而不容易被看懂

问题二：
这只是我们代码中的一个小逻辑，如果类似的逻辑多，那么每一次都需要判断这些内容，就会倒置我们的
代码特别冗长。
'''
```

总结：

1. if判断式的异常处理只能针对某一段代码，对于不同的代码段的相同类型的错误你需要写重复的if来 进行处理。
2. 在你的程序中频繁的写与程序本身无关，与异常处理有关的if，会使得你的代码可读性极其的差
3. if是可以解决异常的，只是存在1,2的问题，所以，千万不要妄下定论if不能用来异常处理

**python：为每一种异常定制了一个类型，然后提供了一种特定的语法结构用来进行异常处理**

## 基本语法

```python
try:
	被检测的代码块
except 异常类型：
	try中一旦检测到异常，就执行这个位置的逻辑
```

将文件的每一行变成一个迭代器，然后读出来

```python
f = open('a.txt')

g = (line.strip() for line in f)
for line in g:
	print(line)
f.close()
```

但是如果超出了迭代器的范围就会出现 StopIteration 错误

使用异常处理

```python
try:
f = open('a.txt')

    g = (line.strip() for line in f)
    print(next(g))
    print(next(g))
    print(next(g))
    print(next(g))
    print(next(g))
    print(next(g))
    print(next(g))
    print(next(g))
    print(next(g))
    print(next(g))
except StopIteration:
    f.close()
	print('读取出错')
```

## 异常类只能用来处理指定的异常情况

```python
s1 = 'hello'
try:
	int(s1)
except IndexError as e:
	print e
```

## 多分支

主要是用来针对不同的错误情况进行错误处理

```python
s1 = 'hello'
try:
	int(s1)
except IndexError as e:
	print(e)
except KeyError as e:
	print(e)
except ValueError as e:
	print(e)
```

## 万能异常:Exception

```python
s1 = 'hello'
try:
	int(s1)
except Exception as e:
	print(e)
```

## 多分支加万能异常

```python
s1 = 'hello'
try:
	int(s1)
except IndexError as e:
	print(e)
except KeyError as e:
	print(e)
except ValueError as e:
	print(e)
except Exception as e:
	print(e)
```

## 其他异常情况

```python
s1 = '10'
try:
	int(s1)
except IndexError as e:
	print(e)
except KeyError as e:
	print(e)
except ValueError as e:
	print(e)
except Exception as e:
	print(e)
else:
	print('try内代码块没有异常则执行我')
finally:
	print('无论异常与否,都会执行该模块,通常是进行清理工作')

```

## 主动触发异常

```python
try:
	raise TypeError('类型错误')
except Exception as e:
	print(e)
```

## 自定义异常

```python
class EvaException(BaseException):
	def __init__(self,msg):
		self.msg=msg
	def __str__(self):
		return self.msg
try:
	raise EvaException('类型错误')
except EvaException as e:
	print(e)
```

## 断言

表达式位True时，程序继续运行，表达式为False时程序终止运行，并报AssertionError错误

```python
assert 1 == 1
assert 1 == 2
```

## try..except的方式比较if的方式的好处

1. 把错误处理和真正的工作分开来
2. 代码更易组织，更清晰，复杂的工作任务更容易实现
3. 毫无疑问，更安全了，不至于由于一些小的疏忽而使程序意外崩溃了
