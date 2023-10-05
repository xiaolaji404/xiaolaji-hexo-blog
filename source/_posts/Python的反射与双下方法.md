---
title: Python的反射与双下方法
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python的反射与双下方法.jpg
banner_img: /Photo/Blog_Photo/Banner/Python的反射与双下方法.jpg
typora-root-url: Python的反射与双下方法
date: 2022-10-01 00:34
---

# 反射

python面向对象中的反射：通过字符串的形式操作对象相关的属性。python中的一切事物都是对象（都 可以使用反射）

四个可以实现自省的函数

下列方法适用于类和对象（一切皆对象，类本身也是一个对象）

**对对象的反射**

```py
class Foo:
    f = '类的静态变量'
    def __init__(self,name,age):
        self.name=name
        self.age=age

	def say_hi(self):
		print('hi,%s'%self.name)
		
obj=Foo('KD',73)

# 检测是否含有某属性
print(hasattr(obj,'name'))
print(hasattr(obj,'say_hi'))

# 获取属性
n=getattr(obj,'name')
print(n)
func=getattr(obj,'say_hi')
func()

print(getattr(obj,'aaaaaaaa','不存在啊')) # 报错

# 设置属性
setattr(obj,'sb',True)
setattr(obj,'show_name',lambda self:self.name+'sb')
print(obj.__dict__)
print(obj.show_name(obj))

# 删除属性
delattr(obj,'age')
delattr(obj,'show_name')
# delattr(obj,'show_name111') # 不存在,则报错

print(obj.__dict__)
```

**对类的反射**

```py
class Foo(object):
	staticField = "test"

	def __init__(self):
		self.name = '陈松'
	def func(self):
		return 'func'
		
	@staticmethod
	def bar():
		return 'bar'

print(getattr(Foo, 'staticField'))
print(getattr(Foo, 'func'))
print(getattr(Foo, 'bar'))
```

当前模块的反射

```py
import sys

def s1():
	print('s1')

def s2():
	print('s2')

this_module = sys.modules[__name__]

print(hasattr(this_module, 's1'))
print(getattr(this_module, 's2'))
```

其他模块的反射

```py
程序目录：
    module_test.py
    test.py
    
当前文件：
	test.py
```

```py
import module_test as obj

obj.test()

print(hasattr(obj,'test'))

getattr(obj,'test')()
```

举例:

使用反射前

```python
class User:
	def login(self):
		print('欢迎来到登录页面')
		
	def register(self):
		print('欢迎来到注册页面')
	def save(self):
		print('欢迎来到存储页面')

while 1:
	choose = input('>>>').strip()
	if choose == 'login':
        obj = User()
        obj.login()

    elif choose == 'register':
        obj = User()
        obj.register()
        
	elif choose == 'save':
        obj = User()
        obj.save()
```

用了反射之后

```python
class User:
	def login(self):
		print('欢迎来到登录页面')
        
	def register(self):
		print('欢迎来到注册页面')
        
	def save(self):
		print('欢迎来到存储页面')

user = User()
while 1:
	choose = input('>>>').strip()
	if hasattr(user, choose):
		func = getattr(user, choose)
		func()
	else:
		print('输入错误。。。。')
```

# 函数 vs 方法

## 通过打印函数(方法)名确定

```python
def func():
	pass
	
print(func)

class A:
	def func(self):
		pass
		
print(A.func)
obj = A()
print(obj.func)
```

## 通过types模块验证

```python
from types import FunctionType
from types import MethodType

def func():
	pass

class A:
	def func(self):
		pass

obj = A()

print(isinstance(func,FunctionType))
print(isinstance(A.func,FunctionType))
print(isinstance(obj.func,FunctionType))
print(isinstance(obj.func,MethodType))
```

## 静态方法是函数

```python
from types import FunctionType
from types import MethodType

class A:

	def func(self):
		pass

	@classmethod
	def func1(cls):
		pass

	@staticmethod
	def func2(self):
		pass

obj = A()

#类方法是方法
print(isinstance(A.func1,FunctionType))
print(isinstance(obj.func1,FunctionType))
# 静态方法其实是函数
print(isinstance(A.func2,FunctionType))
print(isinstance(obj.func2,FunctionType))
```

## 函数与方法的区别

那么，函数和方法除了上述的不同之处，我们还总结了一下几点区别。

1. 函数的是显式传递数据的。如我们要指明为 len() 函数传递一些要处理数据。
2. 函数则跟对象无关。
3. 方法中的数据则是隐式传递的。
4. 方法可以操作类内部的数据。
5.  方法跟对象是关联的。如我们在用strip()方法是，是不是都是要通过str对象调用，比如我们有字符串s,然后`s.strip()`这样调用。是的，strip()方法属于str对象。

我们或许在日常中会口语化称呼函数和方法时不严谨，但是我们心中要知道二者之间的区别。

在其他语言中，如Java中只有方法，C中只有函数，C++么，则取决于是否在类中

# 双下方法

## `__len__`

```python
class B:
	def __len__(self):
		return 666
		
b = B()
print(len(b)) # len 一个对象就会触发 __len__方法。

class A:
	def __init__(self):
		self.a = 1
		self.b = 2

	def __len__(self):
		return len(self.__dict__)
a = A()
print(len(a))
```

## `__hash__`

```python
class A:
    def __init__(self):
            self.a = 1
            self.b = 2

	def __hash__(self):
		return hash(str(self.a)+str(self.b))
a = A()
print(hash(a))
```

## `__str__`

如果一个类中定义了 `__str__` 方法，那么在打印 对象 时，默认输出该方法的返回值。

```python
class A:
	def __init__(self):
		pass
	def __str__(self):
		return '陈松'
a = A()
print(a)
print('%s' % a)
```

## `__repr__`

如果一个类中定义了 `__repr__` 方法，那么在repr(对象) 时，默认输出该方法的返回值。

```python
class A:
	def __init__(self):
		pass
	def __repr__(self):
		return '陈松'
a = A()
print(repr(a))
print('%r'%a)
```

## `__call__`

对象后面加括号，触发执行。

注：构造方法new的执行是由创建对象触发的，即：对象 = 类名() ；而对于 call 方法的执行是由对象后 加括号触发的，即：对象() 或者 类()()

```python
class Foo:

	def __init__(self):
		print('__init__')
	def __call__(self, *args, **kwargs):
		print('__call__')

obj = Foo() # 执行 __init__
obj() # 执行 __call__
```

## `__eq__`

```python
class A:
	def __init__(self):
        self.a = 1
        self.b = 2
	def __eq__(self,obj):
		if self.a == obj.a and self.b == obj.b:
			return True
a = A()
b = A()
print(a == b)
```

## `__del__`

析构方法，当对象在内存中被释放时，自动触发执行。

注：此方法一般无须定义，因为Python是一门高级语言，程序员在使用时无需关心内存的分配和释放， 因为此工作都是交给Python解释器来执行，所以，析构函数的调用是由解释器在进行垃圾回收时自动触 发执行的。

## `__new__`

1. `__new__()` 方法是在类准备将自身实例化时调用。
2. `__new__()` 方法始终都是类的静态方法，即使没有被加上静态方法装饰器
3. 通常来说，新式类开始实例化时， `__new__()` 方法会返回cls（cls指代当前类）的实例，然后该类 的 `__init__()` 方法作为构造方法会接收这个实例（即self）作为自己的第一个参数，然后依次传 入 `__new__()` 方法中接收的位置参数和命名参数。

```python
class A:
	def __init__(self):
        self.x = 1
        print('in init function')
	def __new__(cls, *args, **kwargs):
        print('in new function')
        return object.__new__(A, *args, **kwargs)
        
a = A()
print(a.x)
```

单例模式

```python
class A:
	__instance = None
	def __new__(cls, *args, **kwargs):
		if cls.__instance is None:
			obj = object.__new__(cls)
			cls.__instance = obj
		return cls.__instance
		
a=A()
b=A()
print(a==b)
```

单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。

**【采用单例模式动机、原因】**

对于系统中的某些类来说，只有一个实例很重要，例如，一个系统中可以存在多个打印任务，但是只能有一个正在工作的任务；一个系统只能有一个窗口管理器或文件系统；一个系统只能有一个计时工具或ID(序号)生成器。如在Windows中就只能打开一个任务管理器。如果不使用机制对窗口对象进行唯一化，将弹出多个窗口，如果这些窗口显示的内容完全一致，则是重复对象，浪费内存资源；如果这些窗口显示的内容不一致，则意味着在某一瞬间系统有多个状态，与实际不符，也会给用户带来误解，不知道哪一个才是真实的状态。因此有时确保系统中某个对象的唯一性即一个类只能有一个实例非常重要。 如何保证一个类只有一个实例并且这个实例易于被访问呢？定义一个全局变量可以确保对象随时都可以被访问，但不能防止我们实例化多个对象。一个更好的解决办法是让类自身负责保存它的唯一实例。这个类可以保证没有其他实例被创建，并且它可以提供一个访问该实例的方法。这就是单例模式的模式动机。

**【单例模式优缺点】**

**【优点】**

一、实例控制

单例模式会阻止其他对象实例化其自己的单例对象的副本，从而确保所有对象都访问唯一实例。

二、灵活性

因为类控制了实例化过程，所以类可以灵活更改实例化过程。

**【缺点】**

一、开销

虽然数量很少，但如果每次对象请求引用时都要检查是否存在类的实例，将仍然需要一些开销。可以通过使用静态初始化解决此问题。

二、可能的开发混淆

使用单例对象（尤其在类库中定义的对象）时，开发人员必须记住自己不能使用new关键字实例化对 象。因为可能无法访问库源代码，因此应用程序开发人员可能会意外发现自己无法直接实例化此类。

三、对象生存期

不能解决删除单个对象的问题。在提供内存管理的语言中（例如基于.NET Framework的语言），只有 单例类能够导致实例被取消分配，因为它包含对该实例的私有引用。在某些语言中（如 C++），其他类 可以删除对象实例，但这样会导致单例类中出现悬浮引用

## `__item__`系列

```python
class Foo:
	def __init__(self, name):
		self.name = name

	def __getitem__(self, item):
		print(self.__dict__[item])

	def __setitem__(self, key, value):
		self.__dict__[key] = value
		print(f'{key}赋值{value}成功')

	def __delitem__(self, key):
		print('del obj[key]时,我执行')
		self.__dict__.pop(key)

	def __delattr__(self, item):
		print('del obj.key时,我执行')
		self.__dict__.pop(item)
    # def __setattr__(self, key, value):
	# print(f'当设置{key}={value}对象的时候执行')
	# def __getattr__(self, item):
	# print("当获取变量的时候执行我")
f1 = Foo('sb')
f1.age=19
f1['age'] = 18
f1['age1'] = 19
del f1.age1
del f1['age']
f1['name'] = 'mingzi'
print(f1.__dict__)
```

## 上下文管理器相关

`__enter__` `__exit__`

```python
class A:

	def __init__(self, text):
		self.text = text

	def __enter__(self): # 开启上下文管理器对象时触发此方法
		self.text = self.text + '您来啦'
		return self # 将实例化的对象返回f1

	def __exit__(self, exc_type, exc_val, exc_tb): # 执行完上下文管理器对象f1时触发此方法
		self.text = self.text + '这就走啦'

with A('大爷') as f1:
	print(f1.text)
print(f1.text)
```

自定义文件管理器

```python
class Diycontextor:
	def __init__(self, name, mode):
        self.name = name
        self.mode = mode

	def __enter__(self):
        print("Hi enter here!!")
        self.filehander = open(self.name, self.mode)
        return self.filehander
	
	def __exit__(self,*args):
        print("Hi exit here")
        self.filehander.close()

with Diycontextor('config', 'r') as f:
	for i in f:
		print(i.strip())
```

案例

```python
class StarkConfig:
	def __init__(self, num):
		self.num = num
	
	def run(self):
		self()

	def __call__(self, *args, **kwargs):
		print(self.num)

class RoleConfig(StarkConfig):
	def __call__(self, *args, **kwargs):
		print(345)

	def __getitem__(self, item):
		return self.num[item]

v1 = RoleConfig('abcedf')
v2 = StarkConfig('2333')
print(v1[3])
# print(v2[2])
v1.run()
```

```python
class UserInfo:
	pass
class Department:
	pass
class StarkConfig:
	def __init__(self, num):
		self.num = num
	
	def changelist(self, request):
		print(self.num, request)
	
	def run(self):
		self.changelist(999)

class RoleConfig(StarkConfig):
	def changelist(self, request):
		print(666, self.num)

class AdminSite:

	def __init__(self):
		self._registry = {}
     def register(self, k, v):
		self._registry[k] = v
        
site = AdminSite()
site.register(UserInfo, StarkConfig)
# 1
obj = site._registry[UserInfo]()

# 2
# obj = site._registry[UserInfo](100)
obj.run()
```

```python
class UserInfo:
	pass
	
class Department:
	pass

class StarkConfig:
	def __init__(self,num):
		self.num = num

	def changelist(self,request):
		print(self.num,request)
	
	def run(self):
		self.changelist(999)

class RoleConfig(StarkConfig):
	def changelist(self,request):
		print(666,self.num)

class AdminSite:

	def __init__(self):
		self._registry = {}

	def register(self,k,v):
		self._registry[k] = v(k)

site = AdminSite()
site.register(UserInfo,StarkConfig)
site.register(Department,RoleConfig)

for k,row in site._registry.items():
row.run()
class A:
	list_display = []
    
	def get_list(self):
		self.list_display.insert(0, 33)
		return self.list_display

s1 = A()
print(s1.get_list())

```

```python
class A:
	list_display = [1, 2, 3]
	def __init__(self):
		self.list_display = []
	def get_list(self):
        self.list_display.insert(0, 33)
        return self.list_display
        
s1 = A()
print(s1.get_list())
```

```python
class A:
	list_display = []

	def get_list(self):
		self.list_display.insert(0,33)
		return self.list_display

class B(A):
	list_display = [11,22]

s1 = A()
s2 = B()
print(s1.get_list())
print(s2.get_list())
```
