---
title: Python中的继承
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python中的继承.jpg
banner_img: /Photo/Blog_Photo/Banner/Python中的继承.jpg
typora-root-url: Python中的继承
date: 2022-10-01 00:20
---

# 面向对象的继承

**面向对象三大特性**

**封装** 根据 **职责** 将 **属性** 和 **方法 封装** 到一个抽象的 **类** 中

**继承 实现代码的重用**，相同的代码不需要重复的编写

**多态** 不同的对象调用相同的方法，产生不同的执行结果，**增加代码的灵活度**

不用继承创建对象

```python
class Person:
	def __init__(self,name,sex,age):
        self.name = name
        self.age = age
        self.sex = sex

class Cat:
	def __init__(self,name,sex,age):
        self.name = name
        self.age = age
        self.sex = sex

class Dog:
	def __init__(self,name,sex,age):
        self.name = name
        self.age = age
        self.sex = sex
```

使用继承的方式

```python
class Aniaml(object):
	def __init__(self,name,sex,age):
        self.name = name
        self.age = age
        self.sex = sex

class Person(Aniaml):
	pass

class Cat(Aniaml):
    pass

class Dog(Aniaml):
	pass
```

**继承的概念：****子类** 拥有 **父类** 的所有 **方法 和 属性**

![是否使用继承进行对比](image-20220709204526823.png)

继承的优点也是显而易见的：

1. 增加了类的耦合性（耦合性不宜多，宜精）。
2. 减少了重复代码。
3. 使得代码更加规范化，合理化。

# 继承的分类

上面的那个例子，涉及到的专业术语：

- Dog 类是 Animal 类的子类， Animal 类是 Dog 类的父类， Dog 类从 Animal 类继承
- Dog 类是 Animal 类的派生类， Animal 类是 Dog 类的基类， Dog 类从 Animal 类派生

继承：可以分**单继承**，**多继承**。

python3x版本中只有一种类：

python3中使⽤的都是新式类. 如果基类谁都不继承. 那这个类会默认继承 object

## 单继承

### 类名，对象执行父类方法

```python
class Aniaml(object):
	type_name = '动物类'

	def __init__(self,name,sex,age):
        self.name = name
        self.age = age
        self.sex = sex

	def eat(self):
		print('吃',self)
		
class Person(Aniaml):
    pass

class Cat(Aniaml):
	pass

class Dog(Aniaml):
	pass

print(Person.type_name)
Person.eat('东西')
p1 = Person('aaron','男',18)
print(p1.__dict__)
print(p1.type_name)
p1.type_name = '666'
print(p1)
p1.eat()
```

### 执行顺序

```python
class Aniaml(object):
	type_name = '动物类'

	def __init__(self,name,sex,age):
        self.name = name
        self.age = age
        self.sex = sex

	def eat(self):
		print('吃',self)

class Person(Aniaml):

	def eat(self):
		print('%s 用筷子吃饭'%self.name)

class Cat(Aniaml):
	pass

class Dog(Aniaml):
	pass

p1 = Person('eagle','男',18)
p1.eat()
```

## 同时执行类以及父类方法

方法一：如果想执行父类的func方法，这个方法并且子类中引用，那么就在子类的方法中写上：父 类.func(对象,其他参数)

```python
class Aniaml(object):
    type_name = '动物类'
    def __init__(self,name,sex,age):
        self.name = name
        self.age = age
        self.sex = sex
    
    def eat(self):
		print('吃东西')

class Person(Aniaml):
	def __init__(self,name,sex,age,mind):
		Aniaml.__init__(self,name,sex,age)
		self.mind = mind

	def eat(self):
		Aniaml.eat(111)
		print('%s 吃饭'%self.name)

class Cat(Aniaml):
	pass

class Dog(Aniaml):
	pass

p1 = Person('aaron','男',18,'想吃东西')
p1.eat()

```

方法二：利用super，super().func(参数)

```python
class Aniaml(object):
    type_name = '动物类'
    def __init__(self,name,sex,age):
        self.name = name
        self.age = age
        self.sex = sex

	def eat(self):
		print('吃东西')

class Person(Aniaml):
	def __init__(self,name,sex,age,mind):
		# super(Person,self).__init__(name,sex,age)
		super().__init__(name,sex,age)
		self.mind = mind

	def eat(self):
		super().eat()
		print('%s 吃饭'%self.name)

class Cat(Aniaml):
	pass
class Dog(Aniaml):
	pass

p1 = Person('aaron','男',18,'想吃东西')
p1.eat()
```

单继承练习题

```python
class Base:
	def __init__(self,num):
		self.num = num
	def func1(self):
		print(self.num)

class Foo(Base):
	pass

obj = Foo(123)
obj.func1()
# 运⾏的是Base中的func1
```

```python
class Base:
	def __init__(self,num):
		self.num = num
	def func1(self):
		print(self.num)

class Foo(Base):
	def func1(self):
		print("Foo.func1",self.num)

obj = Foo(123)
obj.func1()
# 运⾏的是Foo中的func1
```

```python
class Base:
	def __init__(self, num):
		self.num = num
	def func1(self):
		print(self.num)
		self.func2()
	def func2(self):
		print("Base.func2")
class Foo(Base):
	def func2(self):
		print("Foo.func2")

obj = Foo(123)
obj.func1()
# func1是Base中的 func2是⼦类中的
```

```python
class Base:
	def __init__(self, num):
		self.num = num
	def func1(self):
		print(self.num)
		self.func2()
	def func2(self):
		print(111, self.num)

class Foo(Base):
	def func2(self):
		print(222, self.num)

lst = [Base(1), Base(2), Foo(3)]
for obj in lst:
	obj.func2()
```

```python
class Base:
	def __init__(self, num):
		self.num = num
	def func1(self):
		print(self.num)
		self.func2()
	def func2(self):
		print(111, self.num)
class Foo(Base):
	def func2(self):
		print(222, self.num)

lst = [Base(1), Base(2), Foo(3)]
for obj in lst:
	obj.func1()
```

## 方法的重写

- 如果在开发中，**父类的方法实现** 和 **子类的方法实现**，**完全不同**
- 就可以使用 **覆盖** 的方式，**在子类中 重新编写** 父类的方法实现

> 具体的实现方式，就相当于在 **子类中** 定义了一个 **和父类同名的方法并且实现**

重写之后，在运行时，**只会调用** 子类中重写的方法，而不再会调用 **父类封装的方法**

## 对父类方法进行 扩展

- 如果在开发中，子类的方法实现中包含父类的方法实现
  - 父类原本封装的方法实现 是 子类方法的一部分
- 就可以使用扩展的方式

1. **在子类中 重写** 父类的方法
2. 在需要的位置使用 `super().父类方法` 来调用父类方法的执行
3. 代码其他的位置针对子类的需求，编写 **子类特有的代码实现**

关于 super

1. 在 Python 中 super 是一个 特殊的类
2. super() 就是使用 super 类创建出来的对象
3. 最常 使用的场景就是在 重写父类方法时，调用 在父类中封装的方法实现

调用父类方法的另外一种方式（知道）

> 在 Python 2.x 时，如果需要调用父类的方法，还可以使用以下方式：

```
父类名.方法(self)
```

- 这种方式，目前在 Python 3.x 还支持这种方式
- 这种方法 不推荐使用，因为一旦 父类发生变化，方法调用位置的 类名 同样需要修改

提示

- 在开发时， 父类名 和 super() 两种方式不要混用
- 如果使用当前子类名调用方法，会形成递归调用，出现死循环

## 父类的 私有属性 和 私有方法

- 子类对象 不能 在自己的方法内部，直接 访问 父类的 私有属性 或 私有方法
- 子类对象 可以通过 父类 的 公有方法 间接 访问到 私有属性 或 私有方法

1. 子类对象 不能 在自己的方法内部，直接 访问 父类的 私有属性 或 私有方法
2. 子类对象 可以通过 父类 的 公有方法 间接 访问到 私有属性 或 私有方法

> 私有属性、方法 是对象的隐私，不对外公开，外界 以及 子类 都不能直接访问
>
> 私有属性、方法 通常用于做一些内部的事情

![示意图](image-20220709211548852.png)

- B 的对象不能直接访问 __num2 属性
- B 的对象不能在 demo 方法内访问 __num2 属性
- B 的对象可以在 demo 方法内，调用父类的 test 方法
- 父类的 test 方法内部，能够访问 __num2 属性和 __test 方法

```python
class A:
	def __init__(self,num1,num2):
        self.num1=num1
        self.__num2=num2
	def test(self):
        self.__test()
        print(self.__num2)
        print("父类公有方法")
	def __test(self):
        print("父类私有方法")
class B(A):
    def demo(self):
		print("子类")
b1 =B(10,20)
print(b1.num1)
# print(b1.__num2)
b1.test()
# b1.__test()
print(b1.__dict__)
print(b1._A__num2) # 不推荐使用
b1._A__test() #不推荐使用

```

# 多继承

**概念**

**子类** 可以拥有 **多个父类**，并且具有 **所有父类** 的 **属性** 和 **方法**

例如：**孩子** 会继承自己 **父亲** 和 **母亲** 的 **特性**

![image-20220710085319316](/image-20220710085319316.png)

**语法**

```python
class 子类名(父类名1, 父类名2...)
	pass
```

**问题的提出**

如果 **不同的父类** 中存在 **同名的方法**，**子类对象** 在调用方法时，会调用 **哪一个父类中**的方法呢？

提示：**开发时，应该尽量避免这种容易产生混淆的情况！** —— 如果 **父类之间** 存在 **同名的属性或者方法**，应该 **尽量避免**使用多继承

![关系图](image-20220710085552103.png)

```python
class ShenXian: # 神仙
	def fei(self):
		print("神仙都会⻜")
	def chitao(self):
		print("吃蟠桃")
class Monkey: # 猴
	def chitao(self):
		print("猴⼦喜欢吃桃⼦")
class SunWukong(ShenXian, Monkey): # 孙悟空是神仙, 同时也是⼀只猴 先继承哪个就先执行哪个类的同名方法
	pass
	
sxz = SunWukong() # 孙悟空
sxz.chitao() # 会吃桃⼦
sxz.fei() # 会⻜

print(SunWukong.__mro__)
```

## 经典类的多继承

```python
class A:
	pass
class B(A):
	pass
class C(A):
	pass
class D(B, C):
	pass
class E:
	pass
class F(D, E):
	pass
class G(F, D):
	pass
class H:
	pass
class Foo(H, G):
	pass
```

画图

![关系图](image-20220710085843154.png)

在经典类中采⽤的是深度优先，遍历⽅案. 什么是深度优先. 就是⼀条路走到头. 然后再回来. 继续找下⼀ 个.

类的MRO(method resolution order): Foo-> H -> G -> F -> E -> D -> B -> A -> C.

## 新式类的多继承

### mro序列

MRO是一个有序列表L，在类被创建时就计算出来。

通用计算公式为：

```
mro(Child(Base1，Base2)) = [ Child ] + merge( mro(Base1), mro(Base2), [Base1, Base2] )（其中Child继承自Base1, Base2）
```

如果继承至一个基类：class B(A)  

这时B的mro序列为

```
mro( B ) = mro( B(A) )
= [B] + merge( mro(A) + [A] )
= [B] + merge( [A] + [A] )
= [B,A]
```

如果继承至多个基类：class B(A1, A2, A3 …) 

这时B的mro序列

```
mro(B) = mro( B(A1, A2, A3 …) )
= [B] + merge( mro(A1), mro(A2), mro(A3) ..., [A1, A2, A3] )
= ...
```

计算结果为列表，列表中至少有一个元素即类自己，如上述示例[A1,A2,A3]。merge操作是C3算法的核心。

### 表头和表尾

表头：列表的第一个元素

表尾：列表中表头以外的元素集合（可以为空）

示例：列表：[A, B, C] 表头是A，表尾是B和C

### 列表之间的+操作

[A] + [B] = [A, B]

merge操作示例：

如计算merge( [E,O], [C,E,F,O], [C] )

有三个列表 ： ① ② ③

```
1 merge不为空，取出第一个列表列表①的表头E，进行判断
各个列表的表尾分别是[O], [E,F,O]，E在这些表尾的集合中，因而跳过当前当前列表
2 取出列表②的表头C，进行判断
C不在各个列表的集合中，因而将C拿出到merge外，并从所有表头删除
merge( [E,O], [C,E,F,O], [C]) = [C] + merge( [E,O], [E,F,O] )
3 进行下一次新的merge操作 ......
---------------------
```

![关系图](image-20220710090525745.png)

计算mro(A)方式：

```
mro(A) = mro( A(B,C) )

原式= [A] + merge( mro(B),mro(C),[B,C] )

mro(B) = mro( B(D,E) )
        = [B] + merge( mro(D), mro(E), [D,E] ) # 多继承
        = [B] + merge( [D,O] , [E,O] , [D,E] ) # 单继承mro(D(O))=[D,O]
        = [B,D] + merge( [O] , [E,O] , [E] ) # 拿出并删除D
        = [B,D,E] + merge([O] , [O])
        = [B,D,E,O]
        
mro(C) = mro( C(E,F) )
        = [C] + merge( mro(E), mro(F), [E,F] )
        = [C] + merge( [E,O] , [F,O] , [E,F] )
        = [C,E] + merge( [O] , [F,O] , [F] ) # 跳过O，拿出并删除
        = [C,E,F] + merge([O] , [O])
        = [C,E,F,O]
        
原式= [A] + merge( [B,D,E,O], [C,E,F,O], [B,C])
    = [A,B] + merge( [D,E,O], [C,E,F,O], [C])
    = [A,B,D] + merge( [E,O], [C,E,F,O], [C]) # 跳过E
    = [A,B,D,C] + merge([E,O], [E,F,O])
    = [A,B,D,C,E] + merge([O], [F,O]) # 跳过O
    = [A,B,D,C,E,F] + merge([O], [O])
    = [A,B,D,C,E,F,O]
```

**python面向对象的三大特性：继承，封装，多态**

1. **封装:** 把很多数据封装到⼀个对象中. 把固定功能的代码封装到⼀个代码块, 函数, 对象, 打包成模块. 这都属于封装的思想. 具体的情况具体分析. 比如. 你写了⼀个很⽜B的函数. 那这个也可以被称为封 装. 在⾯向对象思想中. 是把⼀些看似⽆关紧要的内容组合到⼀起统⼀进⾏存储和使⽤. 这就是封装.
2. **继承:** ⼦类可以⾃动拥有⽗类中除了私有属性外的其他所有内容. 说⽩了, ⼉⼦可以随便⽤爹的东⻄. 但是朋友们, ⼀定要认清楚⼀个事情. 必须先有爹, 后有⼉⼦. 顺序不能乱, 在python中实现继承非常 简单. 在声明类的时候, 在类名后⾯添加⼀个⼩括号,就可以完成继承关系. 那么什么情况可以使⽤继 承呢? 单纯的从代码层⾯上来看. 两个类具有相同的功能或者特征的时候. 可以采⽤继承的形式. 提取 ⼀个⽗类, 这个⽗类中编写着两个类相同的部分. 然后两个类分别取继承这个类就可以了. 这样写的 好处是我们可以避免写很多重复的功能和代码. 如果从语义中去分析的话. 会简单很多. 如果语境中 出现了x是⼀种y. 这时, y是⼀种泛化的概念. x比y更加具体. 那这时x就是y的⼦类. 比如. 猫是⼀种动 物. 猫继承动物. 动物能动. 猫也能动. 这时猫在创建的时候就有了动物的"动"这个属性. 再比如, ⽩骨 精是⼀个妖怪. 妖怪天⽣就有⼀个比较不好的功能叫"吃⼈", ⽩骨精⼀出⽣就知道如何"吃⼈". 此时 ⽩骨精继承妖怪.
3. **多态:** 同⼀个对象, 多种形态. 这个在python中其实是很不容易说明⽩的. 因为我们⼀直在⽤. 只是没 有具体的说. 比如. 我们创建⼀个变量a = 10 , 我们知道此时a是整数类型. 但是我们可以通过程序让a = "hello", 这时, a⼜变成了字符串类型. 这是我们都知道的. 但是, 我要告诉你的是. 这个就是多态性. 同⼀个变量a可以是多种形态。
