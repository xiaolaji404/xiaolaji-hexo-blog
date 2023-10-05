---
title: Python的迭代器与生成器
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python的迭代器与生成器.jpg
banner_img: /Photo/Blog_Photo/Banner/Python的迭代器与生成器.jpg
typora-root-url: Python的迭代器与生成器
date: 2022-09-30 23:50
---

# 迭代器

迭代是访问集合元素的一种方式。迭代器是一个可以记住遍历的位置的对象。迭代器对象从集合的第一 个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

## 可迭代对象

我们已经知道可以对list、tuple、str等类型的数据使用for...in...的循环语法从其中依次拿到数据进行使 用，我们把这样的过程称为遍历，也叫迭代。

但是，如果将整形或者其他不可迭代的对象进行迭代，就会发生报错。

##  如何判断一个对象是否可以迭代

可以使用 **isinstance()** 判断一个对象是否是 **Iterable** 对象：

```python
# 字符串、列表、元组、字典、集合都可以被for循环，说明他们都是可迭代的
from collections.abc import Iterable

l = [1, 2, 3, 4]
t = (1, 2, 3, 4)
d = {1: 2, 3: 4}
s = {1, 2, 3, 4}

print(isinstance(l, Iterable))
print(isinstance(t, Iterable))
print(isinstance(d, Iterable))
print(isinstance(s, Iterable))
#如果是可以迭代的，会有布尔返回值
```

## 可迭代对象的本质

我们分析对可迭代对象进行迭代使用的过程，发现每迭代一次（即在for...in...中每循环一次）都会返回对象中的下一条数据，一直向后读取数据直到迭代了所有数据后结束。那么，在这个过程中就应该有一 个“人”去记录每次访问到了第几条数据，以便每次迭代都可以返回下一条数据。我们把这个能帮助我们 进行数据迭代的“人”称为迭代器(Iterator)。

可迭代对象的本质就是可以向我们提供一个这样的中间“人”即迭代器帮助我们对其进行迭代遍历使用。

可迭代对象通过 `__iter__`方法向我们提供一个迭代器，我们在迭代一个可迭代对象的时候，实际上就 是先获取该对象提供的一个迭代器，然后通过这个迭代器来依次获取对象中的每一个数据.

那么也就是说，一个具备了 `__iter__` 方法的对象，就是一个可迭代对象。

可以被迭代要满足的要求就叫做可迭代协议。可迭代协议的定义非常简单，就是内部实现了__iter__方 法。

```python
l = [1, 2, 3, 4]
t = (1, 2, 3, 4)
d = {1: 2, 3: 4}
s = {1, 2, 3, 4}
#dir方法可以查看一个对象的所有属性和方法
print(dir(l))
print(dir(t))
print(dir(d))
print(dir(s))
```

可迭代的：内部必须含有一个__iter__方法

## `__item__函数与__next__函数`

迭代器遵循迭代器协议：必须拥有iter方法和next方法。

list、tuple等都是可迭代对象，我们可以通过iter()函数获取这些可迭代对象的迭代器。然后我们可以对 获取到的迭代器不断使用next()函数来获取下一条数据。iter()函数实际上就是调用了可迭代对象的 `__iter__` 方法。

```python
l = [1, 2, 3, 4]
l_iter = l.__iter__()
item = l_iter.__next__()
print(item)
item = l_iter.__next__()
print(item)
item = l_iter.__next__()
print(item)
item = l_iter.__next__()
print(item)
item = l_iter.__next__()
print(item)
```

for循环，能遍历一个可迭代对象，他的内部到底进行了什么？ 

将可迭代对象转化成迭代器。（可迭代对象.`__iter__`()） 内部使用`__next__`方法，一个一个取值。 加了异常处理功能，取值到底后自动停止。

```py
l = [1, 2, 3, 4]
l_iter = l.__iter__()
while True:
try:
	item = l_iter.__next__()
	print(item)
except StopIteration:
	break
```

对于迭代器使用他的next方法，即在迭代器后面加上 `__next__`,这样就可以通过迭代器的next获取其返回的其记录的下一个位置的数据。所以，想要构造一个迭代器，就要实现他的`__next__`方法，python要求迭代器本身也是可迭代的，所以我们还要为迭代器实现 `__iter__` 方法，而 `__iter__` 方法要返回一个迭代器，迭代器自身正是一个迭代器，所以迭代器的 `__iter__` 方法返回自身即可。

## 如何判断一个对象是否是迭代器

可以使用 isinstance() 判断一个对象是否是 Iterator 对象：

```python
from collections import Iterator

isinstance([], Iterator)
False

isinstance(iter([]), Iterator)
True

isinstance(iter("abc"), Iterator)
True
```

## 为什么要有for循环

for循环就是基于迭代器协议提供了一个统一的可以遍历所有对象的方法，即在遍历之前，先调用对象的 `__iter__`方法将其转换成一个迭代器，然后使用迭代器协议去实现循环访问，这样所有的对象就都可以通 过for循环来遍历了

最重要的一点，转化成迭代器，在循环时，同一时刻在内存中只出现一条数据，极大限度的节省了内存

for item in Iterable 循环的本质就是先通过iter()函数获取可迭代对象Iterable的迭代器，然后对获取到 的迭代器不断调用next()方法来获取下一个值并将其赋值给item，当遇到StopIteration的异常后循环结 束。

# 生成器

## 初识生成器

### Python中提供的生成器

1.  生成器函数：常规函数定义，但是，使用yield语句而不是return语句返回结果。yield语句一次返 回一个结果，在每个结果中间，挂起函数的状态，以便下次从它离开的地方继续执行
2. 生成器表达式：类似于列表推导，但是，生成器返回按需产生结果的一个对象，而不是一次构建一 个结果列表

### 生成器Generator

- 本质：迭代器(所以自带了iter方法和next方法，不需要我们去实现)，他本身是符合迭代器的所有特性的，但是也迭代器的用途与功能不同
- 特点：惰性运算,开发者自定义（可以通过开发者自己的算法每次给出不同的值，让其迭代每次返回值不同）

## 生成器函数

**一个包含yield关键字的函数就是一个生成器函数。**yield与return有类似的作用，都可以的返回一个值给上层，但是return会将当前的程序直接终止，而yield的作用是将该程序暂时挂起，这样再次引用这个函数时会从上一次停止的yield再次开始

每一次获取这个可迭代对象的值，就能推动函数的执行，获取新的返回值。直到函数执行结束。

```python
def genrator_func1():
	a = 1
	print('将a赋值')
	yield a
	b = 2
	print('将b赋值')
	yield b
	
g1 = genrator_func1()
print(g1)
print(g1.__next__())
print(next(g1))
```

## 生成器包子案例

生成器不会一下子在内存中生成太多数据

比如我想卖包子，让包子工厂开始加工10000个包子，但是如果一下子全部生产好，没地方放，而且容 易坏。 那么可以让包子工厂在我需要的时候再生产

```python
def produce():
	'''生产包子'''
	for i in range(10000):
		yield '生产了第%s个包子'%i
		
produce_g = produce()
print(produce_g.__next__())
print(produce_g.__next__())
print(produce_g.__next__())

# 需要一批包子

for i in range(10):#生成器也可以一次性多次生成
print(produce_g.__next__())
```

## 总结

- 使用了yield关键字的函数不再是函数，而是生成器。（使用了yield的函数就是生成器）
- yield关键字有两点作用
  - 保存当前运行状态（断点），然后暂停执行，即将生成器（函数）挂起
  - 将yield关键字后面表达式的值作为返回值返回，此时可以理解为起到了return的作用
- 可以使用next()函数让生成器从断点处继续执行，即唤醒生成器（函数）

## send

send 获取下一个值的效果和next基本一致，但是是一个特殊的next，在执行next的功能后还会给上一个yield传递一个数据

注意事项：

- 第一次使用生成器的时候 是用next获取下一个值
- 最后一个yield不能接受外部的值

```python
def generator():
	print(123)
	content = yield 1
	print('=========',content)
	print(456)
	yield 2
	
g = generator()
ret = g.__next__()
print('***',ret)
ret = g.send('hello')#这里会将hello的值传回去
print('***',ret)
```
