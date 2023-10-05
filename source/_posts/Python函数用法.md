---
title: Python函数用法
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python函数用法.jpg
banner_img: /Photo/Blog_Photo/Banner/Python函数用法.jpg
typora-root-url: Python函数用法
date: 2022-09-30 23:15
---

# 数字比较案例

```python
num1 = int(input("输入第一个数字:"))
num2 = int(input("输入第二个数字:"))
if num1>num2:
	print(num1+">"+num2)
elif num1 < num2:
	print(num1+"<"+num2)
else:
	print(num1+"="+num2)
	
def compare_num(num1,num2):
	if num1>num2:
		print(num1+">"+num2)
	elif num1 < num2:
		print(num1+"<"+num2)
	else:
p		rint(num1+"="+num2)
compare_num(10,20)
```

# 函数的定义与调用

```python
def my_len(s):#定义一个函数
	length = 0
	for i in s:
		length = length + 1
	print(length)
    
my_len("hello world")#直接进行调用
```

```
定义：def 关键词开头，空格之后接函数名称和圆括号()，最后还有一个":"。

def 是固定的，不能变，他就是定义函数的关键字。

空格 为了将def关键字和函数名分开，必须空(四声)，当然你可以空2格、3格或者你想空多少都行，但正常人还是空1格。

函数名：函数名只能包含字符串、下划线和数字且不能以数字开头。虽然函数名可以随便起，但我们给函数起名字还是要尽量简短，并能表达函数功能

括号：是必须加的，先别问为啥要有括号，总之加上括号就对了！

注释：每一个函数都应该对功能和参数进行相应的说明，应该写在函数下面第一行。以增强代码的可读性。

调用：就是 函数名() 要记得加上括号。
```

# 函数的返回值

- return 是一个关键字，这个词翻译过来就是“返回”，所以我们管写在return后面的值叫“返回值”。
- 不写return的情况下，会默认返回一个None
- 一旦遇到return，结束整个函数。
- 返回多个值会被组织成元组被返回，也可以用多个值来接收

```python
def my_len(s):
	length = 0
	for i in s:
		length += 1
	return length
	
ret = my_len('hello world!')
print(ret)
```

实际的要交给函数的内容，简称实参。 

在定义函数的时候它只是一个形式，表示这里有一个参数，简称形参。

1. 按照位置传值：位置参数

   ```python
   def maxnumber(x,y):
   	the_max = x if x > y else y
   	return the_max
   	
   ret = maxnumber(10,20)
   print(ret)
   ```

2. 按照关键字传值：关键字参数。

   ```python
   def maxnumber(x,y):
   	the_max = x if x > y else y
   	return the_max
   	
   ret = maxnumber(y = 10,x = 20)
   print(ret)
   ```

3. 位置、关键字形式混着用：混合传参。

   ```python
   def maxnumber(x,y):
   	the_max = x if x > y else y
   	return the_max
   	
   ret = maxnumber(10,y = 20)
   print(ret)
   ```

   位置参数必须在关键字参数的前面 

   对于一个形参只能赋值一次

4. 默认参数

   ```python
   def stu_info(name,age = 18):
   	print(name,age)
       
   stu_info('aaron')
   stu_info('song',50)
   ```

5.  默认参数是一个可变数据类型

   ```python
   def demo(a,l = []):
   	l.append(a)
   	print(l)
   	
   demo('abc')
   demo('123')
   ```

6. 动态参数

   ```python
   def demo(*args,**kwargs):
   	print(args,type(args))
   	print(kwargs,type(kwargs))
   	
   demo('aaron',1,3,[1,3,2,2],{'a':123,'b':321},country='china',b=1)
   
   #动态参数，也叫不定长传参，就是你需要传给函数的参数很多，不定个数，那这种情况下，你就用*args，**kwargs接收，args是元祖形式，接收除去键值对以外的所有参数，kwargs接收的只是键值对的参数，并保存在字典中。
   ```

   python中函数的参数有位置参数、默认参数、可变参数、命名关键字参数和关键字参数,这个顺序也是定 义函数时的必须顺序。

# 命名空间和作用域

代码在运行伊始，创建的存储“变量名与值的关系”的空间叫做全局命名空间； 在函数的运行中开辟的临时的空间叫做局部命名空间。

命名空间一共分为三种：



- 内置命名空间：

  	系统自带的命名空间

- 全局命名空间

  	本模块中的命名空间

- 局部命名空间

  	函数内部的命名空间

## 变量取值的顺序

在局部调用：局部命名空间-->全局命名空间-->内置命名空间

在全局调用：局部名称空间只在局部范围生效，如果在全局使用局部所定义的变量，会导致报错，因为在全局内没有这个变量

## globals与locals方法

```python
print(globals())#打印目前的全局内的所有变量（不包含局部变量）
print(locals())#打印目前定义的局部变量（除去所有的全局变量）
#实际上全局与局部的所有变量都是严格分开在不同的空间内，分开存放的

def func():
	a = 12
	b = 20
	print(globals())
	print(locals())
	
func()#调用func函数
```

### global 关键字

1.  声明一个全局变量。
2. 在局部作用域想要对全局作用域的全局变量进行修改时，需要用到 global(限于字符串，数字)。

```python
def func():
	global a#在局部定义了一个全局的变量a
	a = 3#对这个全局的变量a进行赋值
	
func()
print(a)

count = 1
def search():
	global count#在局部对一个全局变量进行引用并进行赋值
	count = 2
    
search()#引用函数，在局部中修改成功
print(count)#在全局中未修改
```

**对可变数据类型（list，dict，set）可以直接引用不用通过global**

```python
li = [1,2,3]#这个列表为全局变量
dic = {'name':'aaron'}#这个字典为全局变量

def change():
	li.append(4)#在局部中对列表进行了修改
	dic['age'] = 18#在局部中对字典进行了修改
	print(dic)
	print(li)
	
change()#函数中修改成功 
print(dic)#同时外部也被修改成功
print(li)#同时外部也被修改成功
```

## nonlocal

1. 不能修改全局变量。
2.  在局部作用域中，对父级作用域（或者更外层作用域非全局作用域）的变量进行引用和修改，并且引用的哪层，从那层及以下此变量全部发生改变。

```python
def add_b():
	b = 1
	def do_global():
		b = 10#此刻外层函数中b=1，但是内层函数中b=10
		print(b)#打印结果b=10
		def dd_nolocal():
			nonlocal b # 应用了上一层的变量b
			b = b + 20
            #由于使用了nonlocal函数导致这里的b被修改后上一层的b以及多有下面的所有b都发生改变
			print(b) # 函数中的b确实发生了改变
		dd_nolocal() # 调用函数，导致do_global的命名空间b也改变了
		print(b)#发现由于在子空间中的对变量的改变导致了父空间中的变量也改变
	do_global()
	print(b)#但是变化不会影响最外层的变量，所以这里的b还是1
    
#执行语句，要学会在Python中通过缩进分辨出执行语句与定义的函数 
add_b() # 最上面一层没有变化
```

# 函数的嵌套和作用域链

## 嵌套调用

```python
def mymax(x,y):#定义一个比较两个数大小的函数
	m = x if x > y else y
	return m#返回比较大的那个数
	
def maxx(a,b,c,d):#定义一个新函数，用于比较四个数的大小
	res1 = mymax(a,b)#嵌套调用先比较前两个数选出最大的
	res2 = mymax(res1,c)#利用前面比较出的结果，比较出是否有更大的
	res3 = mymax(res2,d)#利用前面比较出的结果，比较出是否有更大的
	return res3#最终返回最大值
	
ret = maxx(23,453,12,-13)
print(ret)
```

## 嵌套申明

```python
def f1():
	print("in f1")
	def f2():#在一个函数的申明中再定义一个函数，叫做嵌套申明
		print("in f2")
	f2()
	
f1()
```

# 函数名的本质

函数名的本质实际是内存地址

函数名所具备的特性：

1. 可以被引用

   ```python
   def func():
   	print('in func')
   	
   f = func
   
   print(f)#打印出来的是这个函数所存放的地址
   f()
   ```

   

2. 可以被当做容器类型的元素

   ```python
   def f1():
   	print('f1')
   	
   def f2():
   	print('f2')
   	
   def f3():
   	print('f3')
   	
   l = [f1,f2,f3]
   d = {'f1':f1,'f2':f2,'f3':f3}
   
   #调用
   l[0]()
   d['f2']()
   #将函数的名字放在容器中进行调用
   #可行原因：函数名本身是函数所在的地址，这样就可以将这个地址当做一个量进行存放
   ```

   

3. 可以当做函数的参数和返回值

```python
def f1():#定义一个叫做f1的函数
	print('f1')

def func(argv):#定义一个函数，参数是一个函数的地址
	argv()
	return argv#作用是返回这个函数，当使用一个变量去接收这个函数的结果时，就将这个变量也变成了这个函数，那么这个接收的量就可以变成一个相同的函数，与其他函数的使用方法一致
	
f = func(f1)
f()
```

# 闭包

## 闭包的定义

```python
def func():
	name = '张三'
	def inner():
		print(name)
		return inner
	
f = func()
f()
```

内部函数包含对外部作用域而非全局作用域变量的引用，该内部函数称为闭包函数

解释：如果一个内部函数，引用了一个变量，而这个变量不是其内部的，而是一个外部的，而且不是全局变量，那么称这个函数是一个闭包函数

## 用于判断是否为闭包的函数 closure

```python
def func():
	name = 'aaron'
	def inner():
		print(name)
	print(inner.__closure__)
	return inner
		
f = func()#innner函数引用了name变量，这个变量是func内的，但是并非全局变量，是闭包
f()

(<cell at 0x000002103F7A5678: str object at 0x000002103F76E180>,)
aaron
# 最后运行的结果里面有cell就是闭包

---------------------------------------分割线-----------------------------------------------

name = 'aaron'
def func():
	def inner():
		print(name)#引用了外部元素，但是这个元素是全局变量，所以不是闭包
	print(inner.__closure__)
	return inner
	
f = func()
f()

None
aaron
# 输出结果为None，说明不是闭包

---------------------------------------分割线-----------------------------------------------
def func():
	name = 'aaron'
	def inner():
		nonlocal name
		name='lisi'
		print(name)#引用了父级函数中的name，且不是全局变量，所以是闭包
	print(inner.__closure__)
	return inner
	
f = func()
f()
#输出结果为cell说明是闭包

---------------------------------------分割线-----------------------------------------------
def func(a,b):
	def inner(x):
		return a*x + b#这里使用了外部函数所获得的参数，也算是使用了外部函数的变量
	print(inner.__closure__)
	return inner

func1 = func(4,5)
func2 = func(7,8)
print(func1(5),func2(6))

#使用内部函数使用外部函数的参数，也算使用变量，也是闭包

```

```python
from urllib.request import urlopen#从urllib.request引用一个包叫做urlopen


def func():
	content = urlopen('http://myip.ipip.net').read()#访问这个网址并将其内容进行读取
	
    def get_content():
		print(get_content.__closure__)
		return content#最后返回的是其父空间的变量，因为这个所以这个也是闭包
		
return get_content#返回的是函数名而不是结果，此时contant中已经有了网站中的内容


code = func() #get_content()  这个code的表示的是函数get_content的函数地址，而不是量
content = code()#相当于调用了get_content这个函数
print(content.decode('utf-8'))

content2 = code()
print(content2.decode('utf-8'))
#内部函数直接return外部函数的参数，也是使用变量，也是闭包
```
