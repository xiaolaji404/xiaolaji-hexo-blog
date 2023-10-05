---
title: Python中类的成员
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python中类的成员.jpg
banner_img: /Photo/Blog_Photo/Banner/Python中类的成员.jpg
typora-root-url: Python中类的成员
date: 2022-10-01 00:33
---

# 细分类的组成成员

之前咱们讲过类大致分两块区域

```python
class A:
	name = 'KD'

# 第一部分:静态字段(静态变量)部分(这一部分调用了类自己本身，表示了类自己的自身属性)

	def __init__(self):
		pass
	def func(self):
		pass

# 第二部分:方法部分（这一部分表示了类可以实施的方法，可以 自己或其他进行操作）
```

每个区域详细划分

```python
class A:#在方法名前面带__的属于私有 

    company_name = 'KD' # 静态变量(静态字段)
    __iphone = '132333xxxx' # 私有静态变量(私有静态字段)

	def __init__(self,name,age): #特殊方法（在类的定有里，有部分的固定的字段方法）
        self.name = name #对象属性(普通字段)
        self.__age = age # 私有对象属性(私有普通字段)

    def func1(self): # 普通方法
        pass

    def __func(self): #私有方法
        
        
        print(666)

    @classmethod # 类方法
        def class_func(cls):
            """ 定义类方法，至少有一个cls参数 """
            print('类方法')

	@staticmethod #静态方法
		def static_func():
			""" 定义静态方法 ，无默认参数"""
			print('静态方法')
    @property # 属性
        def prop(self):
            pass
```

类的私有成员

对于每一个类的成员而言都有两种形式：

- 公有成员，在任何地方都能访问
- 私有成员，只有在类的内部才能方法

**私有成员和公有成员的访问限制不同**：

静态字段(静态属性

- 公有静态字段：类可以访问；类内部可以访问；派生类中可以访问
- 私有静态字段：仅类内部可以访问；

公有静态字段访问范围示例

```python
class C:

	name = "公有静态字段"
	
	def func(self):
		print (C.name)

class D(C):
	def show(self):
		print (C.name)

print(C.name) # 类访问

obj = C()
obj.func() # 类内部可以访问

obj_son = D()
obj_son.show() # 派生类中可以访问
```

私有静态字段访问示例

```python
class C:
	__name = "私有静态字段"
	
	def func(self):
		print (C.__name)

class D(C):
	def show(self):
		print (C.__name)

print(C.__name) # 不可在外部访问

obj = C()
print(C.__name) # 不可在外部访问
obj.func() # 类内部可以访问

obj_son = D()
obj_son.show() #不可在派生类中可以访问
```

普通字段(对象属性)

公有普通字段：对象可以访问；类内部可以访问；派生类中可以访问

私有普通字段：仅类内部可以访问；

公有普通字段示例

```python
class C:

	def __init__(self):
		self.foo = "公有字段"

	def func(self):
		print(self.foo) # 类内部访问

class D(C):

	def show(self):
		print(self.foo) # 派生类中访问

obj = C()

obj.foo # 通过对象访问
obj.func() # 类内部访问

obj_son = D();
obj_son.show() # 派生类中访问
```

私有普通字段示例

```python
class C:

	def __init__(self):
		self.__foo = "私有字段"

	def func(self):
		print self.foo # 类内部访问

class D(C):

	def show(self):
		print self.foo ＃ 派生类中访问

obj = C()

obj.__foo # 通过对象访问 ==> 错误
obj.func() # 类内部访问 ==> 正确

obj_son = D();
obj_son.show() # 派生类中访问 ==> 错误
```

```python
class C:

	def __init__(self):
		self.__foo = "私有字段"
	
	def func(self):
		print(self.__foo) # 类内部访问
       
class D(C):
    
	def show(self):
		print(self.__foo) # 派生类中访问

obj = C()

print(obj.__foo) # 通过对象访问 ==> 错误
obj.func() # 类内部访问 ==> 正确

obj_son = D()
obj_son.show() # 派生类中访问 ==> 错误    
```

方法:

- 公有方法:对象可以访问；类内部可以访问；派生类中可以访问
- 私有方法:仅类内部可以访问；

共有方法示例

```python
class C:

	def __init__(self):
		pass

	def add(self):
		print('in C')

class D(C):

	def show(self):
		print('in D')

	def func(self):
		self.show()

obj = D()
obj.show() # 通过对象访问
obj.func() # 类内部访问
obj.add() # 派生类中访问
```

私有方法示例

```python
class C:

	def __init__(self):
		pass

	def __add(self):
		print('in C')

class D(C):

	def __show(self):
		print('in D')
    def func(self):
		self.__show()

obj = D()
obj.__show() # 通过不能对象访问
obj.func() # 类内部可以访问
obj.__add() # 派生类中不能访问
```

总结

对于这些私有成员来说,他们只能在类的内部使用,不能再类的外部以及派生类中使用.

**ps：非要访问私有成员的话，可以通过 对象._类__属性名,但是绝对不允许!!!**

为什么可以通过.类__私有成员名访问呢?因为类在创建时,如果遇到了私有成员(包括私有静态字段,私有普 通字段,私有方法)它会将其保存在内存时自动在前面加上类名.

# 类的其他成员

这里的其他成员主要就是类方法：

方法包括：普通方法、静态方法和类方法，三种方法在**内存中都归属于类**，区别在于调用方式不同。

**实例方法**

```
定义：第一个参数必须是实例对象，该参数名一般约定为“self”，通过它来传递实例的属性和方法（也可以传类的属性和方法）；

调用：只能由实例对象调用。
```

类方法

```
定义：使用装饰器@classmethod。第一个参数必须是当前类对象，该参数名一般约定为“cls”，通过它来传递类的属性和方法（不能传实例的属性和方法）；

调用：实例对象和类对象都可以调用。
```

静态方法

```
定义：使用装饰器@staticmethod。参数随意，没有“self”和“cls”参数，但是方法体中不能使用类或
实例的任何属性和方法；

调用：实例对象和类对象都可以调用。
```

双下方法（后面会讲到）

定义：双下方法是特殊方法，他是解释器提供的 由双下划线加方法名加双下划线 方法名的具有特殊意 义的方法,双下方法主要是python源码程序员使用的，我们在开发中尽量不要使用双下方法，但是深入研究双下方法，更有益于我们阅读源码。 调用：不同的双下方法有不同的触发方式，就好比盗墓时触发的机关一样，不知不觉就触发了双下方 法，例如：`init`

## 类方法

使用装饰器@`classmethod`。

原则上，类方法是将类本身作为对象进行操作的方法。假设有个方法，且这个方法在逻辑上采用类本身 作为对象来调用更合理，那么这个方法就可以定义为类方法。另外，如果需要继承，也可以定义为类方 法。

如下场景：

假设我有一个学生类和一个班级类，想要实现的功能为： 

执行班级人数增加的操作、获得班级的总人数； 

学生类继承自班级类，每实例化一个学生，班级人数都能增加； 

最后，我想定义一些学生，获得班级中的总人数。

**思考：**这个问题用类方法做比较合适，为什么？因为我实例化的是学生，但是如果我从学生这一个实例 中获得班级总人数，在逻辑上显然是不合理的。同时，如果想要获得班级总人数，如果生成一个班级的 实例也是没有必要的。

```python
class Student:
	__num = 0
	
	def __init__(self, name, age):
		self.name = name
		self.age = age
		Student.addNum() # 写在__new__方法中比较合适，但是现在还没有学，暂且放到这里

@classmethod
def addNum(cls):
	cls.__num += 1

@classmethod
def getNum(cls):
	return cls.__num

Student('KD', 18)
Student('KK', 36)
Student('DD', 73)
print(Student.getNum())
```

## 静态方法

使用装饰器`@staticmethod`。

静态方法是类中的函数，不需要实例。静态方法主要是用来存放逻辑性的代码，逻辑上属于类，但是和 类本身没有关系，也就是说在静态方法中，不会涉及到类中的属性和方法的操作。可以理解为，静态方 法是个独立的、单纯的函数，它仅仅托管于某个类的名称空间中，便于使用和维护。

譬如，我想定义一个关于时间操作的类，其中有一个获取当前时间的函数。

```python
import time

class TimeTest(object):
	def __init__(self, hour, minute, second):
		self.hour = hour
		self.minute = minute
		self.second = second

@staticmethod
	def showTime():
		return time.strftime("%H:%M:%S", time.localtime())

print(TimeTest.showTime())
t = TimeTest(2, 10, 10)
nowTime = t.showTime()
print(nowTime)
```

## 方法综合案例

需求

1. 设计一个 Game 类

2. 属性：

   定义一个 **类属性** top_score 记录游戏的 历史最高分

   定义一个 **实例属性** player_name 记录 当前游戏的玩家姓名

3. 方法：

   静态方法 show_help 显示游戏帮助信息

   类方法 show_top_score 显示历史最高分

   实例方法 start_game 开始当前玩家的游戏

4. 主程序步骤

   1.查看帮助信息

   2.查看历史最高分

   3.创建游戏对象，开始游戏

![示意图](image-20220711121734992.png)

### 案例小结

1. 实例方法—— 方法内部需要访问实例属性

   		实例方法 内部可以使用 类名. 访问类属性

2. 类方法 —— 方法内部 只 需要访问 类属性

3. 静态方法 —— 方法内部，不需要访问 实例属性 和 类属性

**提问**

应该定义 **实例方法**

因为，**类只有一个**，在 **实例方法** 内部可以使用 **类名**. 访问类属性

```python
class Game(object):

	# 游戏最高分，类属性
	top_score = 0

	@staticmethod
	def show_help():
		print("帮助信息：让僵尸走进房间")

	@classmethod
	def show_top_score(cls):
		print("游戏最高分是 %d" % cls.top_score)
	
	def __init__(self, player_name):
		self.player_name = player_name
	
	def start_game(self):
		print("[%s] 开始游戏..." % self.player_name)

    # 使用类名.修改历史最高分
    Game.top_score = 999

# 1. 查看游戏帮助
Game.show_help()

# 2. 查看游戏最高分
Game.show_top_score()

# 3. 创建游戏对象，开始游戏
game = Game("小明")

game.start_game()

# 4. 游戏结束，查看游戏最高分
Game.show_top_score()
```

## 属性

property是一种特殊的属性，访问它时会执行一段功能（函数）然后返回值

```
例一：BMI指数（bmi是计算而来的，但很明显它听起来像是一个属性而非方法，如果我们将其做成一个
属性，更便于理解）

成人的BMI数值：
过轻：低于18.5
正常：18.5-23.9
过重：24-27
肥胖：28-32
非常肥胖, 高于32
    体质指数（BMI）=体重（kg）÷身高^2（m）
    EX：70kg÷（1.75×1.75）=22.86
```

```python
class People:
	def __init__(self,name,weight,height):
        self.name=name
        self.weight=weight
        self.height=height
	@property
	def bmi(self):
		return self.weight / (self.height**2)

p1=People('陈松',75,1.85)
print(p1.bmi)
```

将一个类的函数定义成特性以后，对象再去使用的时候obj.name,根本无法察觉自己的name是执行了一 个函数然后计算出来的，这种特性的使用方式遵循了**统一访问的原则**

由于新式类中具有三种访问方式，我们可以根据他们几个属性的访问特点，分别将三个方法定义为对同 一个属性：获取、修改、删除

```python
class Foo:
	@property
	def AAA(self):
		print('get的时候运行我啊')
		
    @AAA.setter
    def AAA(self,value):
    	print('set的时候运行我啊')

    @AAA.deleter
    def AAA(self):
    	print('delete的时候运行我啊')
    	
#只有在属性AAA定义property后才能定义AAA.setter,AAA.deleter
f1=Foo()
f1.AAA
f1.AAA='aaa'
del f1.AAA
```

或者

```python
class Foo:
    def get_AAA(self):
    	print('get的时候运行我啊')

    def set_AAA(self,value):
    	print('set的时候运行我啊')

	def delete_AAA(self):
		print('delete的时候运行我啊')
	AAA=property(get_AAA,set_AAA,delete_AAA) #内置property三个参数与
get,set,delete一一对应

f1=Foo()
f1.AAA
f1.AAA='aaa'
del f1.AAA
```

商品的例子

```python
class Goods(object):

	def __init__(self):
		# 原价
        self.original_price = 100
        # 折扣
        self.discount = 0.8

    @property
    def price(self):
        # 实际价格 = 原价 * 折扣
        new_price = self.original_price * self.discount
        return new_price

    @price.setter
    def price(self, value):
    	self.original_price = value

    @price.deleter
    def price(self):
    	del self.original_price

obj = Goods()
print(obj.price) # 获取商品价格
obj.price = 200 # 修改商品原价
print(obj.price)
del obj.price # 删除商品原价
```

# isinstace 与 issubclass

isinstance(a,b)：判断a是否是b类（或者b类的派生类）实例化的对象

```python
class A:
	pass

class B(A):
	pass
	
obj = B()

print(isinstance(obj,B))
print(isinstance(obj,A))
```

**issubclass(a,b)：** 判断a类是否是b类（或者b的派生类）的派生类

```python
class A:
	pass

class B(A):
	pass

class C(B):
	pass

print(issubclass(B,A))
print(issubclass(C,A))
```

思考：那么 list str tuple dict等这些类与 Iterable类 的关系是什么？

```python
from collections import Iterable

print(isinstance([1,2,3], list)) # True
print(isinstance([1,2,3], Iterable)) # True
print(issubclass(list,Iterable)) # True

# 由上面的例子可得，这些可迭代的数据类型，list str tuple dict等 都是 Iterable的子类。
```
