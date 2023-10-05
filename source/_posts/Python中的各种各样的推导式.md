---
title: Python中的各种各样的推导式
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python中的各种各样的推导式.jpg
banner_img: /Photo/Blog_Photo/Banner/Python中的各种各样的推导式.jpg
typora-root-url: Python中的各种各样的推导式
date: 2022-09-30 23:55
---

# 列表推导式和生成器表达式

```python
l = [i for i in range(10)]
print(l)
l = (i for i in range(10))
print(l)
l1 = ['项目%s'%i for i in range(10)]
print(l1)
```

1. 把列表解析的[]换成()得到的就是生成器表达式（这里描述了生成器表达式的形式）
2. 列表解析与生成器表达式都是一种便利的编程方式，只不过生成器表达式更节省内存，生成器表达式将许多个变量变成一个统一的表达式，在运行时会自动填入
3.  Python不但使用迭代器协议，让for循环变得更加通用。大部分内置函数，也是使用迭代器协议访 问对象的。例如， sum函数是Python的内置函数，该函数使用迭代器协议访问对象，而生成器实 现了迭代器协议，所以，我们可以直接这样计算一系列值的和：

```python
ret = sum(x for x in range(101))
print(ret)
```

# 推导式详细格式

```python
variable = [out_exp_res for out_exp in input_list if out_exp == 2]
out_exp_res: 列表生成元素表达式，可以是有返回值的函数。
for out_exp in input_list： 迭代input_list将out_exp传入out_exp_res表达式中。
if out_exp == 2： 根据条件过滤哪些值可以。
```

# 列表推导式

30以内所有能被3整除的数

```python
multiples = [i for i in range(30) if i % 3 == 0]
print(multiples)
```

30以内所有能被3整除的数的平方

```python
def squared(x):
	return x*x
	
multiples = [squared(i) for i in range(30) if i % 3 == 0]
print(multiples)
```

找到嵌套列表中名字含有两个及以上‘a’的所有名字

```python
fruits = [['peach','Lemon','Pear','avocado','cantaloupe','Banana','Grape'],
		['raisins','plum','apricot','nectarine','orange','papaya']]
		
print([name for lst in fruits for name in lst if name.count('a') >= 2])
```

总结一下，就是在方括号中，先给出这个变量的定义，然后用循环确认出这个变量的需要满足的条件

# 字典推导式

将一个字典的key和value对调

```python
dic1 = {'a':1,'b':2}

dic2 = {dic1[k]: k for k in dic1}
print(dic2)
```

合并大小写对应的value值，将k统一成小写

```python
dic1 = {'a':1,'b':2,'y':1, 'A':4,'Y':9}

dic2 = {key.lower():dic1.get(key.lower(),0)+dic1.get(key.upper(),0) for key
in dic1 }

print(dic2)
```

# 集合推导式

计算列表中每个值的平方，自带去重功能

```python
l = [1,2,3,4,1,-1,-2,3]

squared = {x**2 for x in l}
print(squared)
```

总结一下，就是在方括号中，先给出这个变量的定义，然后用循环确认出这个变量的需要满足的条件
