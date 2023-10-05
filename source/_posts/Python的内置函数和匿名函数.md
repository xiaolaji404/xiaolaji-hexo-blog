---
title: Python的内置函数和匿名函数
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python的内置函数和匿名函数.jpg
banner_img: /Photo/Blog_Photo/Banner/Python的内置函数和匿名函数.jpg
typora-root-url: Python的内置函数和匿名函数
date: 2022-09-30 23:56
---

# 内置函数

截止到python版本3.6.2，现在python一共为我们提供了**68个内置函数**。

| 内置函数      |             |              |            |                |
| ------------- | ----------- | ------------ | ---------- | -------------- |
| abs()         | dict()      | help()       | min()      | setattr()      |
| all()         | cir()       | hex()        | next()     | slice()        |
| any()         | divmod()    | id()         | object()   | sorted()       |
| ascii()       | enumerate() | input()      | oct()      | staticmethod() |
| bin()         | eval()      | int()        | open()     | str()          |
| bool()        | exec()      | isinstance() | ord()      | sum()          |
| bytearray()   | filter()    | issubclass() | pow()      | super()        |
| bytes()       | float()     | iter()       | print()    | tuple()        |
| callable()    | format()    | len()        | property() | type()         |
| chr()         | frozenset() | list()       | range()    | vars()         |
| classmethod() | getattr()   | locals()     | repr()     | zip()          |
| compile()     | globals()   | map()        | reversed() | `__import__()` |
| complex()     | hasattr()   | max()        | round()    |                |
| delattr()     | hash()      | memoryview() | set()      |                |

# 作用域

- locals：函数会以字典的类型返回当前位置的全部局部变量。
- globals：函数以字典的类型返回全部全局变量。

```python
a = 1
b = 2
print(locals())
print(globals())
# 这两个一样，因为是在全局执行的
def func(argv):
	c = 2
	print(locals())#打印所有的局部变量
	print(globals())#打印所有的全局变量
func(3)
```

## 字符串类型代码的执行 eval，exec，complie

1. eval：计算指定表达式的值，并返回最终结果。（几乎没有用

   ```python
   ret = eval('2 + 2')
   print(ret)
   
   n = 20
   ret = eval('n + 23')#可以将表达式形式的计算式子计算结果
   print(ret)
   
   eval('print("Hello world")')
   ```

   

2. exec:执行字符串类型的代码。

```python
s = '''
for i in range(5):
	print(i)
'''

exec(s)#可以将字符串型式的语句读出来并执行
```

1. compile:将字符串类型的代码编译。代码对象能够通过exec语句来执行或者eval()进行求值。
2.  参数 filename：代码文件名称，如果不是从文件读取代码则传递一些可辨认的值。当传入了 source参数时，filename参数传入空字符即可。　
3. 参数model：指定编译代码的种类，可以指定为 ‘exec’,’eval’,’single’。当source中包含流程语句 时，model应指定为‘exec’；当source中只包含一个简单的求值表达式，model应指定为‘eval’；当 source中包含了交互式命令语句，model应指定为'single'。

```python
# 流程语句使用exec
code1 = 'for i in range(5): print(i)'
compile1 = compile(code1,'','exec')

exec(compile1)

# 简单求值表达式用eval
code2 = '1 + 2 + 3'
compile2 = compile(code2,'','eval')
print(eval(compile2))

# 交互语句用single
code3 = 'name = input("please input you name: ")'
compile3 = compile(code3,'','single')

exec(compile3)
print(name)
```

有返回值的字符串形式的代码用eval，没有返回值的字符串形式的代码用exec，一般不用compile。

## 输入输出相关 input，print

- input:函数接受一个标准输入数据，返回为 string 类型。（这就需要输入后常常需要进行强行转换类型）
- print:打印输出。

```python
''' 源码分析
def print(self, *args, sep=' ', end='\n', file=None): # known special caseof print
	"""
	print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
	file: 默认是输出到屏幕，如果设置为文件句柄，输出到文件
	sep: 打印多个值之间的分隔符，默认为空格
	end: 每一次打印的结尾，默认为换行符
	flush: 立即把内容输出到流文件，不作缓存
	"""
'''

print(11,22,33,sep='*')

print(11,22,33,end='')
print(44,55)

with open('log','w',encoding='utf-8') as f:
	print('写入文件',file=f,flush=True)
```

## 内存相关 hash id

- hash：获取一个对象（可哈希对象：int，str，Bool，tuple）的哈希值。

  ```python
  print(hash(12322))
  print(hash('123'))
  print(hash('arg'))
  print(hash('aaron'))
  print(hash(True))
  print(hash(False))
  print(hash((1,2,3)))
  ```

  

- id:用于获取对象的内存地址。

```python
print(id('abc'))
print(id('123'))
```

## 文件操作相关

- open：函数用于打开一个文件，创建一个 file 对象，相关的方法才可以调用它进行读写
- 只有使用文件对象才能进行对这个文件进行相关操作

模块相关`__import__`

- `__import__` ：函数用于动态加载类和函数 。

## 帮助

- help：函数用于查看函数或模块用途的详细说明。

  ```python
  print(help(print))
  ```

## 调用相关

- callable：函数用于检查一个对象是否是可调用的。如果返回True，object仍然可能调用失败；但 如果返回- False，调用对象ojbect绝对不会成功。

```python
print(callable(0))
print(callable('hello'))

def demo1(a, b):
return a + b

print(callable(demo1))

class Demo2:
	def test1(self):
		return 0
		
print(callable(Demo2))

a = Demo2()
print(callable(a))
# 没有实现 __call__, 返回 False
```

## 查看内置属性

- dir：函数不带参数时，返回当前范围内的变量、方法和定义的类型列表；带参数时，返回参数的属 性、方法列表。如果参数包含方法 `__dir__`() ，该方法将被调用。如果参数不包含 `__dir__`() ， 该方法将最大限度地收集参数信息。

```python
print(dir()) # 获得当前模块的属性列表
print(dir([ ])) # 查看列表的方法
```

## 迭代器生成器相关

- range：函数可创建一个整数对象，一般用在 for 循环中。 
- next：内部实际使用了 __next__ 方法，返回迭代器的下一个项目。

```python
# 首先获得Iterator对象:
it = iter([1,2,3,4,5,6])
# 循环
while True:
	try:
		# 获得下一个值
		x = next(it)
		print(x)
	except StopIteration: # 遇到StopIteration就退出循环
	break
```

- iter：函数用来生成迭代器（讲一个可迭代对象，生成迭代器）。

```python
from collections import Iterable
from collections import Iterator

l = [1,2,3,4] # 可迭代对象，但不是迭代器
print(isinstance(l,Iterable))
print(isinstance(l,Iterator))

l1 = iter(l) # 从一个可迭代对象生成迭代器
print(isinstance(l1,Iterable))
print(isinstance(l1,Iterator))
```

## 基础数据类型相关

### 数字相关（14个）

#### 数据类型（4个）

- bool ：用于将给定参数转换为布尔类型，如果没有参数，返回 False。
- int：函数用于将一个字符串或数字转换为整型。

```python
print(int())
print(int('12'))
print(int(3.6))
print(int('0100',base=2)) # 将2进制的 0100 转化成十进制。结果为 4
```

- float：函数用于将整数和字符串转换成浮点数。

```python
print(float(1))
print(float("123"))
```

- complex：函数用于创建一个值为 real + imag * j 的复数或者转化一个字符串或数为复数。如果第 一个参数为字符串，则不需要指定第二个参数。

```python
print(complex(1,2))
print(complex(1))

print(complex("1"))
print(complex("1+2j"))
```

#### 进制转换（3个）：

- bin：将十进制转换成二进制并返回。
- oct：将十进制转化成八进制字符串并返回。 
- hex：将十进制转化成十六进制字符串并返回。

```python
print(bin(10),type(bin(10)))
print(oct(10),type(oct(10)))
print(hex(10),type(hex(10)))
```

#### 数学运算（7）：

- abs：函数返回数字的绝对值。
- divmod：计算除数与被除数的结果，返回一个包含商和余数的元组(a // b, a % b)。
- round：保留浮点数的小数位数，默认保留整数。
- pow：函数是计算x的y次方，如果z在存在，则再对结果进行取模，其结果等效于pow(x,y) %z）

```python
print(abs(-5)) # 5

print(divmod(7,2)) # (3, 1)

print(round(7/3,2)) # 2.33
print(round(7/3)) # 2
print(round(3.32567,3)) # 3.326

print(pow(2,3)) # 8
print(pow(2,3,3)) # 2
```

- sum：对可迭代对象进行求和计算（可设置初始值）。
- min：返回可迭代对象的最小值（可加key，key为函数名，通过函数的规则，返回最小值）。
- max：返回可迭代对象的最大值（可加key，key为函数名，通过函数的规则，返回最大值）。

```python
print(sum([1,2,3]))
print(sum([1,2,3],100))

print(min([1,2,3]))

ret = min([1,2,3,-10],key=abs)
print(ret)

dic = {'a':3,'b':2,'c':1}
print(min(dic,key=lambda x:dic[x]))
# x为dic的key，lambda的返回值（即dic的值进行比较）返回最小的值对应的键

print(max([1,2,3]))

ret = max([1,2,3,-10],key=abs)
print(ret)

dic = {'a':3,'b':2,'c':1}
print(max(dic,key=lambda x:dic[x]))
```

## 数据结构相关（24个）

### 列表和元祖（2个）

- list：将一个可迭代对象转化成列表（如果是字典，默认将key作为列表的元素）。
- tuple：将一个可迭代对象转化成元祖（如果是字典，默认将key作为元祖的元素）。

```python
l = list((1,2,3))
print(l)
l = list({1,2,3})
print(l)
l = list({'k1':1,'k2':2})
print(l)

tu = tuple((1,2,3))
print(tu)
tu = tuple([1,2,3])
print(tu)
tu = tuple({'k1':1,'k2':2})
print(tu)
```

#### 相关内置函数（2个）

- reversed：将一个序列翻转，并返回此翻转序列的迭代器。
- slice：构造一个切片对象，用于列表的切片。

```python
ite = reversed(['a',2,4,'f',12,6])
for i in ite:
	print(i)

l = ['a','b','c','d','e','f','g']
sli = slice(3)
print(l[sli])

sli = slice(0,7,2)
print(l[sli])
```

#### 字符串相关（9）

- str：将数据转化成字符串
- format:与具体数据相关，用于计算各种小数，精算等。

```python
# 字符串可以提供的参数,指定对齐方式，<是左对齐， >是右对齐，^是居中对齐
print(format('test','<20'))
print(format('test','>20'))
print(format('test','^20'))

# 整形数值可以提供的参数有 'b' 'c' 'd' 'o' 'x' 'X' 'n' None
print(format(192,'b')) # 转换为二进制
print(format(97,'c')) # 转换unicode成字符
print(format(11,'d')) # 转换成10进制
print(format(11,'o')) # 转换为8进制
print(format(11,'x')) # 转换为16进制，小写字母表示
print(format(11,'X')) # 转换为16进制，大写字母表示
print(format(11,'n')) # 和d一样
print(format(11)) # 和d一样

# 浮点数可以提供的参数有 'e' 'E' 'f' 'F' 'g' 'G' 'n' '%' None
print(format(314159265,'e')) # 科学计数法，默认保留6位小数
print(format(314159265,'0.2e')) # 科学计数法，保留2位小数
print(format(314159265,'0.2E')) # 科学计数法，保留2位小数,大写E
print(format(3.14159265,'f')) # 小数点计数法，默认保留6位小数
print(format(3.14159265,'0.10f')) # 小数点计数法，保留10位小数
print(format(3.14e+10000,'F')) # 小数点计数法，无穷大转换成大小字母

# g的格式化比较特殊，假设p为格式中指定的保留小数位数，先尝试采用科学计数法格式化，得到幂指数exp，如果-4<=exp<p，则采用小数计数法，并保留p-1-exp位小数，否则按小数计数法计数，并按p-1保留小数位数
print(format(0.00003141566,'.1g'))
# p=1,exp=-5 ==》 -4<=exp<p不成立，按科学计数法计数，保留0位小数点
print(format(0.00003141566,'.2g'))
# p=2,exp=-5 ==》 -4<=exp<p不成立，按科学计数法计数，保留1位小数点

print(format(3.1415926777,'.1g'))
# p=1,exp=0 ==》 -4<=exp<p成立，按小数计数法计数，保留0位小数点
print(format(3.1415926777,'.2g'))
# p=2,exp=0 ==》 -4<=exp<p成立，按小数计数法计数，保留1位小数点
print(format(3141.5926777,'.2g'))
# p=2,exp=3 ==》 -4<=exp<p不成立，按科学计数法计数，保留1位小数点

print(format(0.00003141566,'.1n')) # 和g相同
print(format(0.00003141566)) # 和g相同
```

- bytes：用于不同编码之间的转化。

```python
s = '你好'
bs = s.encode('utf-8')
print(bs)
s1 = bs.decode('utf-8')
print(s1)
bs = bytes(s,encoding='utf-8')
print(bs)
b = '你好'.encode('gbk')
b1 = b.decode('gbk')
print(b1.encode('utf-8'))
```

- bytearry：返回一个新字节数组。这个数组里的元素是可变的，并且每个元素的值范围: 0 <= x < 256。

```python
ret = bytearray('aaron',encoding='utf-8')
print(id(ret))
print(ret)
print(ret[0])
ret[0] = 65
print(ret)
print(id(ret))
```

- memoryview: 通过内存查看数据，是指对支持缓冲区协议的数据进行包装，在不需要复制对象基 础上允许Python代码访问。

```python
ret = memoryview(bytes('你好',encoding='utf-8'))
print(len(ret))
print(ret)
print(bytes(ret[:3]).decode('utf-8'))
print(bytes(ret[3:]).decode('utf-8'))
```

- ord:输入字符找该字符编码的位置
- chr:输入位置数字找出其对应的字符
- ascii:是ascii码中的返回该值，不是就返回/u...

```python
# ord 输入字符找该字符编码的位置
print(ord('a'))
print(ord('中'))

# chr 输入位置数字找出其对应的字符
print(chr(97))
print(chr(20013))

# 是ascii码中的返回该值，不是就返回/u...
print(ascii('a'))
print(ascii('中'))
```

- repr:返回一个对象的string形式

```python
name = 'aaron'
print('Hello %r'%name)

str1 = '{"name":"aaron"}'
print(str1)
print(repr(str1))
print(type(str1))
```

### 数据集合（3个）

- dict：创建一个字典。
- set：创建一个集合。
- frozenset：返回一个冻结的集合，冻结后集合不能再添加或删除任何元素。

### 相关内置函数（8个）

- len:返回一个对象中元素的个数。
- sorted：对所有可迭代的对象进行排序操作。

```python
l = [('a',1),('c',3),('d',4),('b',2)]
print(sorted(l,key=lambda x:x[1]))

print(sorted(l,key=lambda x:x[1],reverse=True)) # 降序
```

- enumerate: 用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出 数据和数据下标，一般用在 for 循环当中。

```python
print(enumerate([1,2,3]))

for i in enumerate([1,2,3]):
print(i)

for i in enumerate([1,2,3],100):
print(i)
```

- all：可迭代对象中，全都是True才是True
- any：可迭代对象中，有一个True 就是True

```python
print(all([1,2,True,0]))
print(any([1,'',0]))
```

- zip：函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这 些元组组成的列表。如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同。

```python
l1 = [1,2,3,]
l2 = ['a','b','c',5]
l3 = ('*','**',(1,2,3))
for i in zip(l1,l2,l3):
	print(i)
```

- filter：用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。

```python
def func(x):
	return x%2 == 0
ret = filter(func,[1,2,3,4,5,6,7,8,9,10])
print(ret)
for i in ret:
	print(i)
```

- map:会根据提供的函数对指定序列做映射。Python 3.x 返回迭代器

```python
def square(x):
	return x**2
	
ret1 = map(square,[1,2,3,4,5,6,7,8])
ret2 = map(lambda x:x ** 2,[1,2,3,4,5,6,7,8])
ret3 = map(lambda x,y : x+y,[1,2,3,4,5,6,7,8],[8,7,6,5,4,3,2,1])

for i in ret1:
	print(i,end=' ')
print('')
for i in ret2:
	print(i,end=' ')
print('')
for i in ret3:
	print(i,end=' ')
```

# 匿名函数

匿名函数：为了解决那些功能很简单的需求而设计的一句话函数。

```python
# 这段代码
def calc(n):
	return n ** n
	
	
print(calc(10))

# 换成匿名函数
calc = lambda n: n ** n
print(calc(10))
```

> 匿名函数格式的说明

函数名 = lambda 参数 ：返回值，实参

1. 参数可以有多个，用逗号隔开
2. 匿名函数不管逻辑多复杂，只能写一行，且逻辑执行结束后的内容就是返回
3. 返回值和正常的函数一样可以是任意数据类型

```python
l=[3,2,100,999,213,1111,31121,333]
print(max(l))

dic={'k1':10,'k2':100,'k3':30}

print(max(dic))
print(dic[max(dic,key=lambda k:dic[k])])

res = map(lambda x:x**2,[1,5,7,4,8])
for i in res:
	print(i)
	
res = filter(lambda x:x>10,[5,8,11,9,15])
for i in res:
	print(i)
```
