---
title: Python中的封装与多态
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python中的封装与多态.jpg
banner_img: /Photo/Blog_Photo/Banner/Python中的封装与多态.jpg
typora-root-url: Python中的封装与多态
date: 2022-10-01 00:29
---

# 封装

1. **封装** 是面向对象编程的一大特点
2. 面向对象编程的 **第一步** —— 将 **属性** 和 **方法 封装** 到一个抽象的 **类** 中
3. **外界** 使用 **类** 创建 **对象**，然后 **让对象调用方法**
4. **对象方法的细节** 都被 **封装** 在 **类的内部**

第一步：将内容封装到某处

```python
class Foo:
	def __init__(self,name,age):
        self.name = name
        self.age = age
        
obj1 = Foo('chensong',18)
obj2 = Foo('aaron',16)
```

第二步：从某处调用被封装的内容

```python
class Foo:
	def __init__(self,name,age):
		self.name = name
		self.age = age

	def detail(self):
		print(self.name)
		print(self.age)

obj1 = Foo('chensong',18)
obj2 = Foo('aaron',16)

print(obj1.name)
print(obj2.age)
# 通过对象直接调用被封装的内容

obj1.detail()
obj2.detail()
# 通过self间接调用被封装的内容
```

## 案例一， 摆放家具

**需求**

1. 房子(House)有户型、总面积和家具名称列表

   ​	

   - 新房子没有任何的家具

   

2. 家具(HouseItem)有名字和占地面积，其中

   			 席梦思(bed) 占地 4 平米

      		 	衣柜(chest) 占地 2 平米
   	
      		 	餐桌(table) 占地 1.5 平米

3. 将以上三件 家具 添加 到 房子 中

4. 打印房子时，要求输出：**户型、总面积、剩余面积、家具名称列表**

![实例图](image-20220710130733553-1685188728096-19.png)

**剩余面积**

1. 在创建房子对象时，定义一个 剩余面积的属性，初始值和总面积相等
2. 当调用 add_item 方法，向房间 添加家具 时，让 剩余面积 -= 家具面积

**思考：**应该先开发哪一个类？

**答案 —— 家具类**

1. 家具简单
2. 房子要使用到家具，被使用的类，通常应该先开发

创建家具

```python
class HouseItem:

	def __init__(self, name, area):
        """
        :param name: 家具名称
        :param area: 占地面积
        """
        self.name = name
        self.area = area
        
	def __str__(self):
		return "[%s] 占地面积 %.2f" % (self.name, self.area)

# 1. 创建家具
bed = HouseItem("席梦思", 4)
chest = HouseItem("衣柜", 2)
table = HouseItem("餐桌", 1.5)

print(bed)
print(chest)
print(table
```

创建房间

```python
class House:

	def __init__(self, house_type, area):
        """
        
        :param house_type: 户型
        :param area: 总面积
        """
        self.house_type = house_type
        self.area = area
        # 剩余面积默认和总面积一致
        self.free_area = area
        # 默认没有任何的家具
        self.item_list = []
        
	def __str__(self):
	
        # Python 能够自动的将一对括号内部的代码连接在一起
        return ("户型：%s\n总面积：%.2f[剩余：%.2f]\n家具：%s"% (self.house_type, self.area,self.free_area, self.item_list))

	def add_item(self, item):

		print("要添加 %s" % item)

...

# 2. 创建房子对象
my_home = House("两室一厅", 60)

my_home.add_item(bed)
my_home.add_item(chest)
my_home.add_item(table)

print(my_home)
```

添加家具

- 主程序只负责创建 房子 对象和 家具 对象
- 让房子 对象调用 add_item 方法 将家具添加到房子中
- 面积计算、剩余面积、家具列表等处理都被封装到房子类的内部

## 案例二、士兵突击

**需求**

1. **士兵** **许三多** 有一把 **AK47**
2. **士兵** 可以 **开火**
3. **枪** 能够 **发射** 子弹
4. **枪** 装填 **装填子弹** —— **增加子弹数量**

![示例图](image-20220711092746156-1685188728097-21.png)

**开发枪类**

- shoot 方法需求

  1. 判断是否有子弹，没有子弹无法射击

  1. 使用 print 提示射击，并且输出子弹数量

```python
class Gun:

	def __init__(self, model):

		# 枪的型号
		self.model = model
		# 子弹数量
		self.bullet_count = 0

	def add_bullet(self, count):
		
		self.bullet_count += count

	def shoot(self):

		# 判断是否还有子弹
		if self.bullet_count <= 0:
			print("没有子弹了...")
			return

        # 发射一颗子弹
        self.bullet_count -= 1
        
        print("%s 发射子弹[%d]...突突突" % (self.model, self.bullet_count))

# 创建枪对象
ak47 = Gun("ak47")
ak47.add_bullet(50)
ak47.shoot()
```

- 开发士兵类

> 假设：每一个新兵 都 **没有枪**

**定义没有初始值的属性**

在定义属性时，如果 **不知道设置什么初始值**，可以设置为 None

- None 关键字 表示 什么都没有
- 表示一个 空对象，没有方法和属性，是一个特殊的常量
- 可以将 None 赋值给任何一个变量

fire 方法需求

- 判断是否有枪，没有枪没法冲锋
- 喊一声口号
- 装填子弹
- 射击

```python
class Soldier:

	def __init__(self, name，gun):

        # 姓名
        self.name = name
        # 枪，士兵初始没有枪 None 关键字表示什么都没有
        self.gun = None

	def fire(self):
        
        # 1. 判断士兵是否有枪
        if self.gun is None:
            print("[%s] 还没有枪..." % self.name)

            return

        # 2. 高喊口号
        print("冲啊...[%s]" % self.name)

        # 3. 让枪装填子弹
        self.gun.add_bullet(50)

        # 4. 让枪发射子弹
        self.gun.shoot()
```

```python
xusanduo=Soldier('许三多')
xusanduo.fire()
ak47=Gun('ak47')
xusanduo.gun=ak47
xusanduo.fire()
xusanduo.gun.shoot()
xusanduo.gun.shoot()
xusanduo.gun.shoot()
xusanduo.gun.shoot()
xusanduo.gun.shoot()
xusanduo.gun.shoot()
xusanduo.gun.shoot()
xusanduo.gun.shoot()
xusanduo.gun.add_bullet(50)
xusanduo.gun.shoot()
xusanduo.gun.shoot()
```

# 多态

（多态的含义其实就是子类去继承大部分的功能，但是对于继承过来的功能可以进行改写，来达到相对于父类更加多的功能）

**多态** 不同的 **子类对象** 调用相同的 **父类方法**，产生不同的执行结果

- **多态** 可以 **增加代码的灵活度**
- 以 **继承** 和 **重写父类方法** 为前提
- 是调用方法的技巧，**不会影响到类的内部设计**

![关系表示图](image-20220711093856993-1685188728097-23.png)

## 案例，哮天犬

**需求**

1. 在 Dog 类中封装方法 game

   普通狗只是简单的玩耍

2. 定义 XiaoTianDog 继承自 Dog ，并且重写 game 方法

   哮天犬需要在天上玩耍

3. 定义 Person 类，并且封装一个和狗玩 的方法

   在方法内部，直接让 **狗对象** 调用 game 方法

![示意图](image-20220711094103117-1685188728097-25.png)

**案例小结**

- Person 类中只需要让狗对象调用 game 方法，而不关心具体是什么狗
- game 方法是在 Dog 父类中定义的
- 在程序执行时，传入不同的 狗对象 实参，就会产生不同的执行效果

> 多态 更容易编写出出通用的代码，做出通用的编程，以适应需求的不断变化！

```python
class Dog:
	def __init__(self, name):
		self.name = name

	def game(self):
		print("%s 蹦蹦跳跳的玩耍..." % self.name)

class XiaoTianDog(Dog):

	def game(self):
		print("%s 飞到天上去玩耍..." % self.name)

class Person:

	def __init__(self, name):
		self.name = name
	
	def game_with_dog(self, dog):
		print("%s 和 %s 快乐的玩耍..." % (self.name, dog.name))
		
		# 让狗玩耍
		dog.game()

# 1. 创建一个狗对象
wangcai = Dog("旺财")
xiaotianquan = XiaoTianDog("飞天旺财")
	
# 2. 创建一个小明对象
xiaoming = Person("小明")

# 3. 让小明调用和狗玩的方法
xiaoming.game_with_dog(wangcai)
xiaoming.game_with_dog(xiaotianquan)
```

python中有一句谚语说的好，你看起来像鸭子，那么你就是鸭子。

对于代码上的解释其实很简答：

```python
class A:
	def f1(self):
		print('in A f1')

	def f2(self):
		print('in A f2')

class B:
	def f1(self):
		print('in B f1')
	
	def f2(self):
		print('in B f2')

obj = A()
obj.f1()
obj.f2()

obj2 = B()
obj2.f1()
obj2.f2()
# A 和 B两个类完全没有耦合性，但是在某种意义上他们却统一了一个标准。
# 对相同的功能设定了相同的名字，这样方便开发，这两个方法就可以互成为鸭子类型。

# 这样的例子比比皆是：str tuple list 都有 index方法，这就是统一了规范。
# str bytes 等等 这就是互称为鸭子类型。
```

# 类的约束

（将具有相同的用途的类，可以在定义某个功能的时候在不同的类中将一样的功能进行相同命名，这样在外部进行调用的时候，可以直接使用相同的函数进行一次性调用，这样将选择权交给用户，更为的灵活，详见下面的支付案例）

写一个支付功能

```python
class QQpay:
	def pay(self,money):
		print('使用qq支付%s元' % money)

class Alipay:
	def pay(self,money):
		print('使用阿里支付%s元' % money)

a = Alipay()
a.pay(100)

b = QQpay()
b.pay(200)
```

统一一下付款方式

```python
class QQpay:
	def pay(self,money):
		print('使用qq支付%s元' % money)

class Alipay:
	def pay(self,money):
		print('使用阿里支付%s元' % money)

def pay(obj,money):
	obj.pay(money)

a = Alipay()
b = QQpay()

pay(a,100)
pay(b,200)
```

如果后期添加微信支付，但是没有统一标准，换个程序员就可能写成这样

```python
class QQpay:
	def pay(self,money):
		print('使用qq支付%s元' % money)

class Alipay:
	def pay(self,money):
		print('使用阿里支付%s元' % money)

class Wechatpay:
	def fuqian(self,money):
		print('使用微信支付%s元' % money)

def pay(obj,money):
	print("===============")
	obj.pay(money)

a = Alipay()
b = QQpay()

pay(a,100)
pay(b,200)

c = Wechatpay()
c.fuqian(300)
```

解释：由于WeChat使用的内部付款方式不是与其他两种相同的pay模式，所以无法在外部一次性进行选择，会减少代码的可读性，没有灵活性

**所以此时我们要用到对类的约束，对类的约束有两种：**

1. 提取⽗类. 然后在⽗类中定义好⽅法. 在这个⽅法中什么都不⽤⼲. 就抛⼀个异常就可以了. 这样所有 的⼦类都必须重写这个⽅法. 否则. 访问的时候就会报错.
2. 使⽤元类来描述⽗类. 在元类中给出⼀个抽象⽅法. 这样⼦类就不得不给出抽象⽅法的具体实现. 也 可以起到约束的效果.

先用第一种方法解决问题

```python
#此处通过，首先在所有的函数的父类内部定义一个pay方法，要求子类必须对这个父类方法进行修改，否则就会进行当初程序员自己定义的一个报错，同时开始提示，如：打印“你没有去定义pay方法”
class Payment:
    """
    此类什么都不做，就是制定一个标准，谁继承我，必须定义我里面的方法。
    """
    
    def pay(self,money):
		raise Exception("你没有实现pay方法")

class QQpay(Payment):
	def pay(self,money):
		print('使用qq支付%s元' % money)

class Alipay(Payment):
	def pay(self,money):
		print('使用阿里支付%s元' % money)

class Wechatpay(Payment):
	def fuqian(self,money):
		print('使用微信支付%s元' % money)

def pay(obj,money):
obj.pay(money)
a = Alipay()
b = QQpay()
c = Wechatpay()
pay(a,100)
pay(b,200)
pay(c,300)
```

引入抽象类的概念处理（不建议）

```python
from abc import ABCMeta,abstractmethod
class Payment(metaclass=ABCMeta): # 抽象类 接口类 规范和约束 metaclass指定的是一个元类
    @abstractmethod#在这里加入一个修饰，在这个修饰下面放入需要的函数，这样可以实现与上面一样要求必须定义的功能，如果不进行定义，回引起IndentationError的报错，但是不会像上一方法那样直接进行打印父方法中的东西
    def pay(self):pass # 抽象方法

class Alipay(Payment):
	def pay(self,money):
		print('使用支付宝支付了%s元'%money)

class QQpay(Payment):
	def pay(self,money):
		print('使用qq支付了%s元'%money)

class Wechatpay(Payment):
	# def pay(self,money):
	# print('使用微信支付了%s元'%money)

def pay(a,money):
	a.pay(money)

a = Alipay()
a.pay(100)
pay(a,100) # 归一化设计：不管是哪一个类的对象，都调用同一个函数去完成相似的功能
q = QQpay()
q.pay(100)
pay(q,100)
w = Wechatpay() # 到实例化对象的时候就会报错
pay(w,100)

# 抽象类和接口类做的事情 ：建立规范
# 制定一个类的metaclass是ABCMeta，
# 那么这个类就变成了一个抽象类(接口类)
# 这个类的主要功能就是建立一个规范
```

总结: 约束. 其实就是⽗类对⼦类进⾏约束. ⼦类必须要写xxx⽅法. 在python中约束的⽅式和⽅法有两种: 

1. 使⽤抽象类和抽象⽅法, 由于该⽅案来源是java和c#. 所以使⽤频率还是很少的 
2. 使⽤⼈为抛出异常的⽅案. 并且尽量抛出的是NotImplementError. 这样比较专业, ⽽且错误比较明 确.(推荐

# super()深入了解

super是严格按照类的继承顺序**（mro）**执行！！！

```python
class A:
	def f1(self):
		print('in A f1')

	def f2(self):
		print('in A f2')

class Foo(A):
	def f1(self):
		super().f2()
		print('in A Foo')

obj = Foo()
obj.f1()
```

```python
class A:
	def f1(self):
		print('in A')

class Foo(A):
	def f1(self):
		super().f1()
		print('in Foo')

class Bar(A):
	def f1(self):
		print('in Bar')

class Info(Foo,Bar):
	def f1(self):
		super().f1()
		print('in Info f1')

obj = Info()
obj.f1()

print(Info.mro())
```

super方法可以在继承后，儿子可以去调用父亲的方法使用super方法即可

这个就是super方法的好处，可以让继承后的直接调用继承的内部方法
