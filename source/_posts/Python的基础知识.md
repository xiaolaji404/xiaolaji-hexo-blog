---
title: Python的基础知识
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python的基础知识.jpg
banner_img: /Photo/Blog_Photo/Banner/Python的基础知识.jpg
typora-root-url: Python的基础知识
date: 2022-09-05 12:32
---

# Python的注释

Python的注释分为两种，分别为单行注释和多行注释

单行注释：#被注释的内容

多行注释：'''被注释内容'''   """被注释内容"""

```python
单行注释：#被注释的内容

多行注释：'''被注释内容'''   """被注释内容"""
```

# Python的基础数据类型

 Python与C语言不同

 其数据类型主要有int float str complex（主要类型）

## 查看一个变量的类型方式

使用type(a)函数

```python
a = 2**64
print(type(a)) #type()是查看数据类型的方法
b = 2**60
print(type(b))
```

运行结果

```python
<class 'int'>
<class 'int'>
```

## 布尔值（True，False）

布尔值也叫做布尔类型，总共有两个值，一个为True（真），一个为False（假），一般被用于逻辑判断

```python
a = 3
b = 5
print(a < b, a > b , a != b)
```

运行结果

```python
True False True
```

## 字符串

字符串是在Python中运用最为广泛的数据类型，所有的从键盘读入的数据，默认都是字符串类型，如果需要进行类型的变化，需要使用相关函数强行进行转换，但此时需要注意，如果输入的字符无法被转换，程序会报错。

### 字符串拼接

 ```python
  a = 'eagle '
  b = 'welcome '
  print(b + a,'*' * 3,a * 3)
 ```

将两个字符串进行相加，实际这个操作的名称为字符串的拼接

运行结果

```python
welcome eagle *** eagle eagle eagle
```

### 字符串的索引与切片

```python
a = 'ABCDEFGHIJK'
print(a[0], a[-11])
print(a[3])
print(a[5])
print(a[7])
a = 'ABCDEFGHIJK'
print(a[0:3])
print(a[2:5])
print(a[0:]) #默认到最后
print(a[0:-1]) # -1 是列表中最后一个元素的索引，但是要满足顾头不顾腚的原则，所以取不到K
元素
print(a[0:5:2]) #加步长
print(a[5:0:-2]) #反向加步长
```

在一个字符串中，Python会对字符串自动进行索引，从正序来看，编号为0~9；从逆序来看，编号为-1~10

运行结果

```python
A
D
F
H
ABC
CDE
ABCDEFGHIJK
ABCDEFGHIJ
ACE
FDB
```

### 字符串常用方法

#### 字母大小写的转换

```python
words = "beautiful is better than ugly."
print(words.capitalize()) #首字母大写
print(words.swapcase()) #大小写翻转
print(words.title()) #每个单词的首字母大写
'''
运行结果
Beautiful is better than ugly.
BEAUTIFUL IS BETTER THAN UGLY.
Beautiful Is Better Than Ugly.
'''
```

#### 将符号填充到字符两侧

```python
# 内容居中，总长度，空白处填充
a = "test"
ret = a.center(20,"*")
print(ret)
'''
运行结果

********test********

'''
```

#### 统计字符串中某个元素出现的个数

```python
# 统计字符串中的元素出现的个数
ret = words.count("e",0,30)
print(ret)
'''
运行结果
3
'''


# startswith 判断是否以...开头
# endswith 判断是否以...结尾
a = "aisdjioadoiqwd12313assdj"
print(a.startswith("a"))
print(a.endswith("j"))
print(a.startswith('sdj',2,5))
print(a.endswith('ado',7,10))
'''
运行结果

True
True
True
True

'''


# 寻找字符串中的元素是否存在
print(a.find('sdj',1,10)) # 返回的找到的元素的索引，如果找不到返回-1
print(a.index('sdj',1,10)) # 返回的找到的元素的索引，找不到报错。
'''
运行结果

2
2

'''

# split 以什么分割，最终形成一个列表此列表不含有这个分割的元素。
ret = words.split(' ')
print(ret)
运行结果：
['beautiful', 'is', 'better', 'than', 'ugly.']

ret = words.rsplit(' ',2) # 加数字指定分割次数
#注意！这边由于指定分割次数小于原本的空格数，所以如直接切割，则从左侧切割两次，在函数前加上r，代表从右侧切割，则切割了右侧的两个空格
print(ret)
'''
运行结果
['beautiful is better', 'than', 'ugly.']
'''

# format的三种玩法 格式化输出
print('{} {} {}'.format('aaron',18,'teacher'))
print('{1} {0} {1}'.format('aaron',18,'teacher'))
print('{name} {age} {job}'.format(job='teacher',name='aaron',age=18))

# strip，此函数可以在字符串中删除某个字符
# 如果直接使用strip函数，则会删除字符串中所有的这个字符
# 加入r或者l可以说明删除左侧或者右侧的
a = '****asdasdasd********'
print(a.strip('*'))
print(a.lstrip('*'))
print(a.rstrip('*'))
运行结果
asdasdasd
asdasdasd********
****asdasdasd

# replace  替换函数
print(words.replace('e','a',2)) # 字符串从左向右开始，把e替换成a，一共替换两次

#一下函数为验证字符串内是有什么组成的，如果是，则返回True，如不是，则返回False
print(words.isalnum()) #验证字符串由字母或数字组成
print(words.isalpha()) #验证字符串只由字母组成
print(words.isdigit()) #验证字符串只由数字组成
```

# 基本运算符

## 基本运算符

![基本运算符](image-20220702205111583.png)

## 比较运算

![比较运算](image-20220702205143801.png)

## 赋值运算

![赋值运算](image-20220702205222875.png)

## 逻辑运算

![逻辑运算符](image-20220702205250638.png)

在没有 () 的情况下 not 优先级高于 and，and 优先级高于 or，即优先级关系为()>not>and>or，同一 优先级从左往右计算。 

x or y , x 为真，值就是 x，x 为假，值是 y； x and y, x 为真，值是 y,x 为假，值是 x。

## 成员运算

![成员之间的运算](image-20220702205832489.png)

## Python运算的优先级

![运算符号优先级](image-20220702205907659.png)

# Python的数据类型

```python
#  总结
# 似乎Python使用括号作为标识符将不同的数据类型全部区分开来了
# 对于元祖，使用了圆括号，其内部元素不可发生变化
# 对于列表，使用了方括号，其内部元素可以发生变化
# 对于字典，使用花括号，其种的元素使用冒号进行一一对应反应出一种映射的关系
# 对于集合，使用花括号，其中的元素直接存储，方式与列表相似，但其中的元素不可变更，但其身可以变更
```

## 数据类型的总结

元祖为圆括号表示；列表为方括号表示；字典为大括号表示，但要求{键：值}一一对应；集合为一种特殊类型，将列表使用set进行强制转换，表示时使用花括号直接与列表方式类似

|      | 书写方式      | 可不可变 | 顺序 |
| ---- | ------------- | -------- | ---- |
| 列表 | 方括号[]      | 可变     | 有   |
| 元组 | 圆括号()      | 可变     | 有   |
| 字典 | 花括号{键:值} | 可变     | 有   |
| 集合 | 花括号{}      | 可变     | 没有 |



## 元组tuple（其中的元素内容不可被更改）

元组被称为只读列表，即数据可以被查询，但不能被修改。

元组与列表的区别：元祖与链表的区别所在：即元祖采用的是圆括号将其中的数据类型包含住，但是其中已经定义的数据类型是不可改动的，而列表其中的数据类型是可以被改动的。

tuple其实不可变的是地址空间，如果地址空间里存的是可变的数据类型的话，比如列表就是可变的 

参考博客 https://blog.csdn.net/machi1/article/details/86601119

总结一下，即为元组内部的元素不能被改变，但是如果在元组里面有一个列表，那么列表内的元素是可以被修改的。

## 列表 list

列表相比于字符串，不仅可以储存不同的数据类型，而且可以储存大量数据，32 位 python 的限制是 536870912 个元素,64 位 python 的限制是 1152921504606846975 个元素。而且**列表是有序的，有索引值，可切片**，方便取值。

### 列表的增加元素

```python
li = [1,'a',2,'d',4]

li.insert(0,22) # 按照索引去增加，对0号元素进行赋值
print(li)

li.append('ddd') # 增加到最后在所有的元素最后，在加上一个元素
print(li)

li.extend(['q,a,w']) # 迭代的去增，增加一个字符串
print(li)

li.extend(['q,a,w','das']) # 迭代的去增，增加两个字符串
print(li)
#append与extand最大的区别就在于，append会将内部的所有的东西全部放进去，一整个的
#而extand是将其中的分为一个个字符串，放进去的是字符串
```

运行结果

```python
[22, 1, 'a', 2, 'd', 4]
[22, 1, 'a', 2, 'd', 4, 'ddd']
[22, 1, 'a', 2, 'd', 4, 'ddd', 'q,a,w']
[22, 1, 'a', 2, 'd', 4, 'ddd', 'q,a,w', 'q,a,w', 'das']
```

### 删除操作

```python
li = [1,'a',2,'d',4,5,'f']
a = li.pop(1) # 按照位置去删除，有返回值
print(a)
del li[1:3] # 按照位置去删除，也可切片删除没有返回值（这里指删除1~2个元素，顾头不顾尾）
print(li)
li.remove('f')#删掉字符串"f"
print(li)
li.clear()#清除所有的内容
print(li)
```

运行结果

```python
a
[1, 4, 5, 'f']
[1, 4, 5]
[]
```

### 改动操作

```python
li = [1,'a',2,'d',4,5,'f']
li[1] = 'aaa'#将第二个元素改为...
print(li)
li[2:3] = [3,'e']#将2~3个元素改为...
print (li)
```

运行结果

```python
[1, 'aaa', 2, 'd', 4, 5, 'f']
[1, 'aaa', 3, 'e', 'd', 4, 5, 'f']
```

### 查找操作

使用切片去查，或者循环去查

### 其他操作

```python
li = [1,2,4,5,4,2,4]
print (li.count(4)) # 统计某个元素在列表中出现的次数
print (li.index(2)) # 用于从列表中找出某个值第一个匹配项的索引位置
li.sort() # 用于在原位置对列表进行排序
print (li)
li.reverse() # 将列表中的元素反向存放
print (li)
```

运行结果

```python
3
1
[1, 2, 2, 4, 4, 4, 5]
[5, 4, 4, 4, 2, 2, 1]
```

# 字典 dict

字典是python中唯一的映射类型，采用键值对（key-value）的形式存储数据。可哈希表示key必须是不可变类型，如：数字、字符串、元组。

在python3.6以后字典就是有顺序的了

参考博客 https://www.cnblogs.com/xieqiankun/p/python_dict.html

### 增加操作

```python
dic = {"age":18, "name":"aaron"}
dic['li'] = ["a","b","c"]#在字典中增加一个映射的关系
print(dic)
dic.setdefault('k','v')
# 在字典中添加键值对时，如果指定的键已经存在则不做任何操作,如果原字典中不存在指定的键值对，
则会添加。
print(dic)
dic.setdefault('k','v1')#k已经存在，所以不做任何操作
print(dic)


#运行结果
{'age': 18, 'name': 'aaron', 'li': ['a', 'b', 'c']}
{'age': 18, 'name': 'aaron', 'li': ['a', 'b', 'c'], 'k': 'v'}
{'age': 18, 'name': 'aaron', 'li': ['a', 'b', 'c'], 'k': 'v'}
```

### 删除操作

```python
dic = {"age":18, "name":"aaron"}
dic_pop = dic.pop('age')
# pop根据key删除键值对，并返回对应的值，如果没有key则返回默认返回值
print(dic_pop)
dic_pop = dic.pop('sex','查无此项')
print(dic_pop)
dic['age'] = 18
print(dic)
del dic['name']
print(dic)
dic['name'] = 'demo'
dic_pop = dic.popitem()
# 删除字典中的某个键值对，将删除的键值对以元祖的形式返回，由于3.6之前的版本字典是无序的，所以会随机删除，现在字典是有序的，会固定删除最后一个
print(dic_pop)
dic_clear = dic.clear()
# 清空字典
print(dic,dic_clear)
```

### 更改操作

```python
dic = {"age":18, "name":"aaron", 'sex':'male'}
dic2 = {"age":30, "name":'demo'}

dic2.update(dic)
# 将dic所有的键值对覆盖添加（相同的覆盖，没有的添加）到dic2中
#整个字典的集体性赋值

print(dic2)

dic2['age'] = 30#对单独一个字典进行赋值
print(dic2)
```

### 查找操作

```python
dic = {"age":18, "name":"aaron", 'sex':'male'}

value = dic['name']#查找函数
# 没有会报错
print(value)

value = dic.get('abc','查无此项')
print(value)
```

### 其他操作

```python
dic = {"age":18, "name":"aaron", 'sex':'male'}
for i in dic.items():#for的循环语句
# 将键和值作为元祖列出
print(i)
for key,value in dic.items():
print(key,value)
for i in dic:
# 只是迭代键
print(i)
keys = dic.keys()
print(keys,type(keys))
value = dic.values()
print(value,type(value))
```

## 集合set

集合是无序的，不重复，确定性的数据集合，它里面的元素是可哈希的(不可变类型)，但是集合本身是不可哈希（所以集合做不了字典的键）的。

以下是集合最重要的两点： 去重，把一个列表变成集合，就自动去重了。 关系测试，测试两组数据之前的交集、差集、并集等关系。

### 创建集合

```python
set1 = set({1,2,'barry'})
set2 = {1,2,'barry'}
print(set1,set2)
```

### 集合的增

```python
set1 = {'abc','def',123,'asdas'}
# add()函数的参数只能接收可哈希数据类型，即不可变数据类型，
比如整型、浮点型、元组、字符串
set1.add('qwer')
print(set1)
# 我们使用update()向集合中添加元素时，update接收的参数应该是可迭代的数据类型，比如字符
串、元组、列表、集合、字典。这些都可以向集合中添加元素，但是整型、浮点型不可以。
set1.update('A')
#update：迭代着增加
print(set1)
set1.update('哈哈哈')
print(set1)
set1.update([1,2,3])
print(set1)
```

### 集合的删

```python
set1 = {'abc','def',123,'asdas'}
set1.remove('abc')
print(set1)
set1.pop()
# 随机删除一个数
print(set1)
set1.clear()
# 清空合集，其中的内容删除
print(set1)
del set1
# 删除合集，整个全部删除
print(set1)
```

### 集合的其他操作

```python
#交集（& 或者 intersection）
#取出两个集合共有的元素
set1 = {1,2,3,4,5}
set2 = {3,4,5,6,7}
print(set1 & set2)
print(set1.intersection(set2))
# 列出两个集合中共同拥有的项


#并集（| 或者 union）
#合并两个集合的所有元素
set1 = {1,2,3,4,5}
set2 = {3,4,5,6,7}
print(set1 | set2)
print(set2.union(set1))
# 列出两个集合中所有的项


#差集（- 或者 difference）
#类似于第一个集合减去两个集合共有的元素
set1 = {1,2,3,4,5}
set2 = {3,4,5,6,7}
print(set1 - set2)
print(set1.difference(set2))
# 在set1中删除set2中有的项


#反交集 （^ 或者 symmetric_difference）
#显示集合中不共存的项
set1 = {1,2,3,4,5}
set2 = {3,4,5,6,7}
print(set1 ^ set2)
print(set1.symmetric_difference(set2))
# 显示set1和set2不共存的项
```

### 子集与超集

当一共集合的所有元素都在另一个集合里，则称这个集合是另一个集合的子集，另一个集合是这个集合的超集

是一个判断的函数，返回的是布尔值

```python
set1 = {1,2,3}
set2 = {1,2,3,4,5,6}
print(set1 < set2)
print(set1.issubset(set2)) # 这两个相同，都是说明set1是set2子集。
print(set2 > set1)
print(set2.issuperset(set1)) # 这两个相同，都是说明set2是set1超集
```

### frozenset不可变集合，让集合变成不可变类型

```python
#将一个集合变成不可改变的类型
set1 = {1,2,3,4,5,6}
s = frozenset(set1)
print(s,type(s))
s.add(7) # 不可以修改,会报错
```

# 流程控制之 --if

## f...else ... 可以有多个分支条件

```python
if 条件:
满足条件执行代码
elif 条件:
上面的条件不满足就走这个
elif 条件:
上面的条件不满足就走这个
elif 条件:
上面的条件不满足就走这个
else:
上面所有的条件不满足就走这段
```

```python
# 例3：if语句多个条件
num = 9
if num >= 0 and num <= 10: # 判断值是否在0~10之间
print 'hello'
# 输出结果: hello
num = 10
if num < 0 or num > 10: # 判断值是否在小于0或大于10
print 'hello'
else:
print 'undefine'
# 输出结果: undefine
num = 8
# 判断值是否在0~5或者10~15之间
if (num >= 0 and num <= 5) or (num >= 10 and num <= 15):
print 'hello'
else:
print 'undefine'
# 输出结果: undefine
```

# 流程控制之 --while

## 基本循环

```python
while 条件:
循环体
```

如果条件为真，那么循环体则执行 

如果条件为假，那么循环体不执行

# 循环中止语句

## break

用于完全结束一个循环，跳出循环体执行循环后面的语句

## continue

和 break 有点类似，区别在于 continue 只是终止本次循环，接着还执行后面的循环，break 则完全终止循环 

## while ... else .. 

while 后面的 else 作用是指，当 while 循环正常执行完，中间没有被 break 中止的话，就会执行 else 后面的语句

# 其他（for，enumerate，range）

for循环：用户按照顺序循环可迭代对象的内容。

```python
s = '先帝创业未半而中道崩殂，今天下三分，益州疲弊，此诚危急存亡之秋也。'
for i in s:
print(i)
li = ['甲','乙','丙','丁']
for i in li:
print(i)
dic = {'a':1,'b':2,'c':3}
for k,v in dic.items():
print(k,v)
```

enumerate：枚举，对于一个可迭代的（iterable）/可遍历的对象（如列表、字符串），enumerate将 其组成一个索引序列，利用它可以同时获得索引和值。

运行出来着的是已元祖的形式出现，key是其序列，values为值

```python
li = ['甲','乙','丙','丁']
for i in li:
print(i)
for i in enumerate(li):
print(i)
for index,value in enumerate(li):
print(index,value)
for index,value in enumerate(li,100): #从哪个数字开始索引
print(index,value)
```

range：指定范围，生成指定数字。

使用for循环进行数字的叠加

```python
for i in range(1,10):
print(i)

for i in range(1,10,2): # 步长，从前往后加，隔一个数加一个数
print(i)

for i in range(10,1,-2): # 反向步长，从后往前加
print(i)
```
