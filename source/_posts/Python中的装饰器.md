---
title: Python中的装饰器
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python中的装饰器.jpg
banner_img: /Photo/Blog_Photo/Banner/Python中的装饰器.jpg
typora-root-url: Python中的装饰器
date: 2022-09-30 23:49
---

# 什么是装饰器

让其他函数在不需要做任何代码变动的前提下，增加额外的功能，装饰器的返回值也是一个函数对象。 装饰器的应用场景：比如插入日志，性能测试，事务处理，缓存等等场景。

简单来说，就是将一个定义好的函数，将其函数名传入另外一个函数，在另一个函数中加入其他的功能，最后返回出新的函数的名，让原本的函数名去接收，完成这个原本函数的更新

## 案例

```python
def func1():
	print("in func1")
# 要求调用func1()输出如下内容，并且前提是不动原本的两行代码
# hello world
# in func1
# hello python
```

解决方案

```python
def func2(func):
	def inner():
		print("hello world")#添加的第一个功能
		func()#这个函数有原本需要的函数功能
		print("hello python")#添加的第二个功能
	return inner#外层函数唯一的作用是将这个修改后的函数返回
func1 = func2(func1)#调用func1的函数将其返回值给func1，完成对func1的升级
func1()
```

# 装饰器的形成过程

如果我想测试某个函数的执行时间

```python
import time#引入time这个库，类似C语言的头文件

def func1():
	print('in func1')
	
def timer(func):#这个是一个对func1的修饰器，可以计算出一个程序的运行时间
	def inner():
		start = time.time()#计算程序开始的时间
		func()#运行程序
         time.sleep(1)#为了让结果更明显，让程序睡眠1秒钟
		print(time.time() - start)#将现在的时间减去开始的时间
	return inner
	
func1 = timer(func1) #inner()目前实际是inner的功能
func1()
```

但是如果有多个函数，我都想让你测试他们的执行时间，你每次是不是都得func1 = timer(func1)?这样 还是有点麻烦，因为这些函数的函数名可能是不相同，有func1，func2,graph,等等，所以更简单的方 法，python给你提供了，那就是语法糖。

语法糖的用法是，先定义一个修饰器，例如像上一个算时间的修饰器，搞个语法糖的叫做@timer

将这个语法糖黏在定义的新函数的上方，即可用timer这个修饰器去修饰这个新定义的函数

```python
import time

def timer(func):
	def inner():
		start = time.time()
		func()
		print(time.time() - start)
	return inner
	
@timer#将下面这个函数用上面的修饰器那样修饰
def func1():
	time.sleep(1)
	print('in func1')
	
func1()
```

# 装饰一个带各种参数的函数

```python
import time

def timer(func):
	def inner(*args,**kwargs):#用这样的方式接收所有的变量
		start = time.time()
		func(*args,**kwargs)
		print(time.time() - start)
	return inner
	
@timer
def func1(*args,**kwargs):
	print(args,kwargs)
	
func1('hello world','abc',123,432,name='zhangsan')
```

与原本的装饰方式类似，但是函数需要将变量的类型进行修改

# wraps装饰器

## 查看函数的相关信息

```python
def index():
	'''这是一条注释信息'''
	print('from index')
	
print(index.__doc__) # 查看函数注释
print(index.__name__) # 查看函数名称

加上装饰器
def outer(func):
	def inner():
		'''这里是inner'''
		func()
	return inner
	
	
@outer
def index():
	'''这是一条注释信息'''
	print('from index')
	
print(index.__doc__) # 查看函数注释
print(index.__name__) # 查看函数名称

#运行结果，此处可知，由于修饰器的作用吗，导致函数的基本信息与相应的注释，全部变成了升级后的函数内的注释以及函数名，不在是原来所需要的函数的名字与信息，具体的解决方法就是wraps修饰器
这是一条注释信息
index
这里是inner
inner
```

导入wraps修饰器，可以保留函数本身的属性以及相关的注释

```python
from functools import wraps
def outer(func):
	@wraps(func)#在修饰器种加入wrap函数
	def inner():
		'''这里是inner'''
		func()
	return inner
	
@outer
def index():
	'''这是一条注释信息'''
	print('from index')
print(index.__doc__) # 查看函数注释
print(index.__name__) # 查看函数名称




###不定长参数参数
from functools import wraps
def outer(func):
	@wraps(func)#在修饰器中加入wraps函数
	def inner(*args,**kwargs):
	'''这里是inner'''
	func(*args,**kwargs)
	return inner


@outer
def index():
	'''这是一条注释信息'''
	print('from index')
print(index.__doc__) # 查看函数注释
print(index.__name__) # 查看函数名称
```

wraps修饰器就是在正常的修饰器种加入一个@wraps(形参)，即可保留函数原本的信息

# 带控制参数的装饰器

加上一个outer函数，可以携带一个flag的值，然后控制装饰器是否生效

解释：在修饰糖的后面加入一个变量或者布尔值，在修饰器的逻辑种加入一个判断，如果为True则进行修饰，如果为False则不进行修饰，但是，不管有没有修饰，修饰器已经起作用，所以为了函数的信息不发生变化，必须让wraps修饰器起作用，判断函数归判断函数，必须让wraps修饰器起作用

```python
from functools import wraps
def oouter(flag):
	def outer(func):
		@wraps(func)#加入一个函数的嵌套执行是为了让wraps修饰器起作用
		def inner(*args, **kwargs):
			'''这里是inner'''
			if flag:#加入判断语句，判断具体要不要进行修饰
				print("开始装饰")
				func(*args, **kwargs)
				print("装饰结束")
			else:
				func(*args, **kwargs)
		return inner
	return outer

@oouter(True)#是否进行修饰要靠修饰糖上面的布尔值类型进行判断
def index(a):
	'''这是一条注释信息'''
	print('from index')
	print(a)
	
print(index.__doc__) # 查看函数注释
print(index.__name__) # 查看函数名称
index('abc')
```

# 多个装饰器装饰一个函数

```python
#先装饰距离函数更近的装饰器
def wrapper1(func):
	def inner():
		print('第一个装饰器，在程序运行之前')
		func()
		print('第一个装饰器，在程序运行之后')
	return inner
	
def wrapper2(func):
	def inner():
		print('第二个装饰器，在程序运行之前')
		func()
		print('第二个装饰器，在程序运行之后')
	return inner
	
@wrapper1#第一个修饰糖
@wrapper2#第二个修饰糖
def f():
	print('Hello')
	
f()
```

总结：哪个修饰糖距离更近就先执行哪个，在执行完一个后，这个函数已经发生了变化，将由修饰糖2修饰过的函数交给修饰糖1进行再次修饰，从而得出修饰结果

# 开放封闭原则

软件实体应该是可扩展但是不可修改的。

- 对于扩展是开放的
- 对于修改是封闭的

装饰器完美的遵循了这个开放封闭原则

# 装饰器的主要功能和固定结构

## 本科所学习的知识总结运用

```python
def outer(func):
	def inner(*args,**kwargs):
		'''执行函数之前要做的'''
		re = func(*args,**kwargs)
		'''执行函数之后要做的'''
		return re
	return inner
	
# 下面是加上wraps的固定结构
from functools import wraps

def outer(func):
	@wraps(func)
	def inner(*args,**kwargs)
		'''执行函数之前要做的'''
		re = func(*args,**kwargs)
		'''执行函数之后要做的'''
		return re
	return inner
	
#加上flag
from functools import wraps

def oouter(flag):
	def outer(func):
	@wraps(func)
	def inner(*args,**kwargs)
		if flag:
			'''执行函数之前要做的'''
			re = func(*args,**kwargs)
			'''执行函数之后要做的'''
		else:
			re = func(*args,**kwargs)
		return re
	return inner
```
