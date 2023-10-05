---
title: Python类的空间问题以及类之间的关系
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python类的空间问题以及类之间的关系.jpg
banner_img: /Photo/Blog_Photo/Banner/Python类的空间问题以及类之间的关系.jpg
typora-root-url: Python类的空间问题以及类之间的关系
date: 2022-10-01 00:18
---

# 类的空间问题

## 添加对象属性

```python
class A:
	def __init__(self,name):
		self.name = name

	def func(self,sex):
		self.sex = sex
```

在类外部添加（在类的外部通过万能的点进行类的属性的添加）

```python
class A:
	def __init__(self,name):
		self.name = name

	def func(self,sex):
		self.sex = sexa

obj = A('chensong')
obj.age = 18
print(obj.__dict__)
```

类的内部添加（引用内部的方法，在类的内部添加属性）

```python
class A:
	def __init__(self,name):
		self.name = name

	def func(self,sex):
		self.sex = sex

obj = A('chensong')
obj.func('男')
print(obj.__dict__)
```

总结：对象的属性不仅可以在init里面添加，还可以在类的其他方法或者类的外面添加。

## 添加类的属性

```python
class A:
	def __init__(self,name):
		self.name = name

	def func(self,sex):
		self.sex = sex

	def func1(self):
		A.bbb = self
        
A.aaa = 'test' # 类的外部添加（使用点进行添加）
print(A.__dict__)

A.func1('123') # 类的内部添加（使用类内部的方法进行在内部进行添加）
print(A.__dict__)
```

总结：类的属性不仅可以在类内部添加，还可以在类的外部添加

## 对象如何找到类的属性

对象空间

1. 产生这个对象空间，并有一个类对象指针
2. 执行 __init__ 方法，给对象封装属性

**对象查找属性的顺序：**先从对象空间找 ------> 类空间找 ------> 父类空间找 ------->.....

**类名查找属性的顺序：**先从本类空间找 -------> 父类空间找--------> ........

上面的顺序都是单向不可逆，类名不可能找到对象的属性。

# 类与类之间的关系

类与类中存在以下关系:

1. 依赖关系
2. 关联关系
3. 组合关系
4. 聚合关系
5. 实现关系
6. 继承关系(类的三大特性之一：继承。)

## 依赖关系

例：将大象装进冰箱，需要两个类, ⼀个是⼤象类, ⼀个是冰箱类

```python
class Elphant:
	def __init__(self,name):
		self.name = name
		
	def open(self):
        '''
        开门
        '''
        pass
        
	def close(self):
        '''
        关门
        '''
        pass
        
class Refrigerator:
	def open_door(self):
		print('冰箱门打开了')
		
def close_door(self):
    print('冰箱门关上了')
```

将大象类和冰箱类进行依赖

```python
class Elphant:
	def __init__(self,name):
		self.name = name
		
	def open(self,obj1):
        '''
        开门
        '''
        print(self.name,'要开门了')
        obj1.open_door()
        
	def close(self):
        '''
        关门
        '''
        pass
        
class Refrigerator:
	def open_door(self):
		print(f'{self.name}冰箱门打开了')
		
def close_door(self):
	print(f'{self.name}冰箱门关上了')

elphant1 = Elphant('大象')		
haier = Refrigerator('海尔')
elphant1.open(haier)
```

## 关联,聚合,组合关系

其实这三个在代码上写法是⼀样的. 但是, 从含义上是不⼀样的

1. 关联关系. 两种事物必须是互相关联的. 但是在某些特殊情况下是可以更改和更换的
2. 聚合关系. 属于关联关系中的⼀种特例. 侧重点是xxx和xxx聚合成xxx. 各⾃有各⾃的声明周期. 比如电脑. 电脑⾥有CPU, 硬盘, 内存等等. 电脑挂了. CPU还是好的. 还是完整的个体
3. 组合关系. 属于关联关系中的⼀种特例. 写法上差不多. 组合关系比聚合还要紧密. 比如⼈的⼤脑, ⼼脏, 各个器官. 这些器官组合成⼀个⼈. 这时. ⼈如果挂了. 其他的东⻄也跟着挂了

关联关系

```python
class Boy:
	def __init__(self,name,girlFirend=None):
		self.name = name
		self.girlFriend = girlFirend

	def have_a_dinner(self):
     	if self.girlFriend:
            print('%s 和 %s 一起晚饭'%(self.name,self.girlFriend.name))
        else:
            print('单身狗，吃什么饭')
		
class Girl:
    def __init__(self,name):
		self.name = name
        
b = Boy('日天')
b.have_a_dinner()

b.girlFriend = Girl('如花')
b.have_a_dinner()

gg = Girl('花花')
bb = Boy('songsong',gg)
bb.have_a_dinner()
```

注意. 此时Boy和Girl两个类之间就是关联关系. 两个类的对象紧密联系着. 其中⼀个没有了. 另⼀个就孤单 的不得了. 关联关系, 其实就是 我需要你. 你也属于我

学校和老师之间的关系

```python
class School:

	def __init__(self,name,address):
		self.name = name
		self.address = address
		
class Teacher:

	def __init__(self,name,school):
        self.name = name
        self.school = school
        
s1 = School('北京校区','北京')
s2 = School('上海校区','上海')
s3 = School('深圳校区','深圳')

t1 = Teacher('T1',s1)
t2 = Teacher('T2',s2)
t3 = Teacher('T3',s3)

print(t1.school.name)
print(t2.school.name)
print(t3.school.name)
```

但是学校也是依赖于老师的，所以老师学校应该互相依赖。

```python
class School:
	
	def __init__(self,name,address):
        self.name = name
        self.address = address
        self.teacher_list = []
	def append_teacher(self,teacher):
		self.teacher_list.append(teacher)
		
		
class Teacher:
	def __init__(self,name,school):
        self.name = name
        self.school = school
        
s1 = School('北京校区','北京')
s2 = School('上海校区','上海')
s3 = School('深圳校区','深圳')

t1 = Teacher('T1',s1)
t2 = Teacher('T2',s2)
t3 = Teacher('T3',s3)

s1.append_teacher(t1.name)
s1.append_teacher(t2.name)
s1.append_teacher(t3.name)

print(s1.teacher_list)
```

**组合：将一个类的对象封装到另一个类的对象的属性中，就叫组合**

例：设计一个游戏，让游戏里面的人物互殴

加上一个武器类，让人使用武器攻击

```python
class Gamerole:
	def __init__(self,name,ad,hp,wea=None):
        self.name = name
        self.ad = ad
        self.hp = hp
        self.wea = wea
        
	def attack(self,p1):
		p1.hp -= self.ad
		print('%s攻击%s,%s掉了%s血,还剩%s'%(self.name,p1.name,p1.name,self.ad,p1.hp))
		
		def equip_weapon(self,wea):
            self.wea = wea
            wea.ad += self.ad
            wea.owner_name = self.name
            
class Weapon:
	def __init__(self,name,ad,owner_name = None):
        self.name = name
        self.owner_name = owner_name
        self.ad = ad
	def weapon_attack(self,p2):
        p2.hp = p2.hp - self.ad
        print('%s利用%s攻击了%s，%s还剩%s血(self.owner_name,self.name,p2.name,p2.name,p2.hp))
        
man = Gamerole('人',10,100)
dog = Gamerole('狗',50,100)
stick = Weapon('木棍',40)
              
man.equip_weapon(stick)
stick.weapon_attack(dog)
# 人利用木棍攻击了狗，狗还剩50血
```

## 案例，循环回合制游戏

```python
import time
import random
class Gamerole:
	def __init__(self, name, ad, hp, wea=None):
        self.name = name
        self.ad = ad
        self.hp = hp
        self.wea = wea
        
	def attack(self, p1):
        p1.hp -= self.ad
        print('%s攻击%s,%s掉了%s血,还剩%s' % (self.name, p1.name, p1.name,self.ad, p1.hp))
	
    def equip_weapon(self, wea):
        self.wea = wea
        wea.ad += self.ad
        wea.owner_name = self.name

    class Weapon:
		def __init__(self,name,ad,owner_name = None):
            self.name = name
            self.owner_name = owner_name
            self.ad = ad
		def weapon_attack(self,p2):
            p2.hp = p2.hp - self.ad
            print('%s利用%s攻击了%s，%s还剩%s血'%(self.owner_name,self.name,p2.name,p2.name,p2.hp))
sunwukong = Gamerole("孙悟空", 30, 500)
caocao = Gamerole("曹操", 60, 100)
anqila = Gamerole("安琪拉", 80, 80)

baigujing = Gamerole("白骨精", 35, 450)
guanyu = Gamerole("关羽", 40, 200)
diaochan = Gamerole("貂蝉", 50, 150)

dongxie_list = [sunwukong, caocao, anqila]xidu_list = [baigujing, guanyu, diaochan]

if __name__ == '__main__':
    print("游戏开始加载")
    # 打印一个菜单
    for i in range(0, 101, 2):
        time.sleep(0.1)
        char_num = i // 2
        per_str = '\r%s%% : %s' % (i, '*' * char_num)
        print(per_str, end='')
	print('')
    info = input("游戏加载完毕，输入任意字符开始！")
    # 输出东邪吸毒阵营里的任务角色
    print("东邪阵营".center(20, '*'))
    for i in dongxie_list:
		print(i.name.center(20))
    print("西毒阵营".center(20, '*'))
    for i in xidu_list:
		print(i.name.center(20))
        
    while True:
    # 判断游戏结束的条件是某一方全部阵亡
        if len(dongxie_list) == 0:
        print("西毒阵营胜利！！！")
            break
        if len(xidu_list) == 0:
            print("东邪阵营胜利！")
            break

         # 游戏逻辑，每次随机选择一名角色出击
         index1 = random.randint(0, len(dongxie_list) - 1)
         index2 = random.randint(0, len(xidu_list) - 1)

        # 开始攻击
        time.sleep(0.5)
        role1 = dongxie_list[index1]
        time.sleep(0.5)
        role2 = xidu_list[index2]
        time.sleep(0.5)
        role1.attack(role2)
        role2.attack(role1)

        # 判断是否有英雄阵亡
        if role1.hp <= 0:
            print("%s阵亡！" % role1.name)
            dongxie_list.remove(role1)
        if role2.hp <= 0:
            print("%s阵亡！" % role2.name)
            xidu_list.remove(role2)
```
