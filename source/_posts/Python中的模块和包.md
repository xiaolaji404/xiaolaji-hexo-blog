---
title: Python中的模块和包
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python中的模块和包.jpg
banner_img: /Photo/Blog_Photo/Banner/Python中的模块和包.jpg
typora-root-url: Python中的模块和包
date: 2022-09-30 23:59
---

# 什么是模块

1.  使用python编写的代码（`.py`文件）
2.  已被编译为共享库或DLL的C或C++扩展
3. 包好一组模块的包
4. 使用C编写并链接到python解释器的内置模块

# 为何要使用模块

实现代码和功能的复用

## import 自定义模块my_module.py

文件名my_module.py,模块名my_module

```python
# my_module.py

print('from the my_module.py')

money = 100

def read1():
	print('my_module->read1->money',money)
	
def read2():
	print('my_module->read2 calling read1')
	read1()
	
def change():
	global money
	money=0
```

模块可以包含可执行的语句和函数的定义，这些语句的目的是**初始化模块**，它们只在模块名第一次遇到 导入import语句时才执行（import语句是可以在程序中的任意位置使用的,且针对同一个模块很import 多次,为了防止你重复导入）

**但是为了能够让程序的可读性更强，所以需要在程序的开头表明所有的引入的包和模块**

python的优化手段是：第一次导入后就将模块名加载到内存了，后续的import语句仅是对已经加载大内存中的模块对象增加了一次引用，不会重新执行模块内的语句）

**也就是说引入包只要一次就好**

```python
import my_module
import my_module
import my_module
import my_module

import sys
print(sys.modules)
# sys.modules是一个字典，内部包含模块名与模块对象的映射，该字典决定了导入模块时是否需要重新导入。
```

每个模块都是一个独立的名称空间，定义在这个模块中的函数，把这个模块的名称空间当做全局名称空 间，这样我们在编写自己的模块时，就不用担心我们定义在自己模块中全局变量会在被导入时，与使用者的全局变量冲突

```python
import my_module
money=10
print(my_module.money)
```

```python
import my_module
def read1():
	print('=========')
	
my_module.read1()
```

```python
import my_module

money = 1
my_module.change()
print(money)
print(my_module.money)
```

## 为模块名起别名，相当于m1=1;m2=m1

```python
import my_module as mm

print(mm.money)
```

> 示范用法：

有两中`sql`模块`mysql`和`oracle`，根据用户的输入，选择不同的`sql`功能

```python
# mysql.py
def sqlparse():
	print('from mysql sqlparse')
	
# oracle
def sqlparse():
	print('from oracle sqlparse')
	
# test.py
db_type=input('>>: ')
if db_type == 'mysql':
	import mysql as db
elif db_type == 'oracle':
	import oracle as db
	
db.sqlparse()
```

## 在一行导入多个模块

```python
import sys, os, re
```

## from ... import ...

对比import my_module，会将源文件的名称空间'my_module'带到当前名称空间中，使用时必须是 my_module.名字的方式

而from 语句相当于import，也会创建新的名称空间，但是将my_module中的名字直接导入到当前的名 称空间中，在当前名称空间中，直接使用名字就可以了

```python
from my_module import read1,read2
money = 1000
read1()
# 导入的函数read1，执行时仍然回到my_module.py中寻找全局变量money
```

```python
from my_module import read1,read2
money = 1000
def read1():
	print('*'*10)
	
read2()
# 导入的函数read2，执行时需要调用read1(),仍然回到my_module.py中找read1()
```

```python
from my_module import read1,read2
money = 1000
def read1():
	print('*'*10)

read1()
# 导入的函数read1，被当前位置定义的read1覆盖掉了
```

```python
from my_module import read1 as read

read()
# 也支持as
```

from my_module import * 把my_module中**所有**的**不是以下划线(_)开头的名字都导入到当前位置** 大部分情况下我们的python程序不应该使用这种导入方式，因为*你不知道你导入什么名字，很有可能 会覆盖掉你之前已经定义的名字。而且可读性极其的差，在交互式环境中导入时没有问题。

因为这样的会导致原本的函数被覆盖

```python
.....
__all__ = ['money','read1']
# 这样在另外一个文件中用from my_module import *就这能导入列表中规定的两个名字

# test.py
from my_module import *

print(money)
read1()
read2()
```

注意：如果my_module.py中的名字前加,即money，则from my_module import *,则_money不能被导 入

- 编写好的一个python文件可以有两种用途：

  1. 脚本，一个文件就是整个程序，用来被执行

  1. 模块，文件中存放着一堆功能，用来被导入使用

- python为我们内置了全局变量 `__name__` 
  1. 当文件被当做脚本执行时： `__name__` 等于`__main__`
  2. 当文件被当做模块导入时： `__name__`等于模块名

- 作用：用来控制.`py`文件在不同的应用场景下执行不同的逻辑（或者是在模块文件中测试代码）
  1.  if `__name__` == `__main__`:

```py
def fib(n):
	a, b = 0, 1
	while b < n:
		print(b, end=',')
		a, b = b, a+b
	print()
	
if __name__ == "__main__":
	print(__name__)
	num = input('num :')
	fib(int(num))
```



# 模块的搜索路径

模块的查找顺序是：内存中已经加载的模块->自建模块->`sys.path`路径中包含的模块

1. 在第一次导入某个模块时（比如my_module），会先检查该模块是否已经被加载到内存中（当前 执行文件的名称空间对应的内存），如果有则直接引用 

   	ps：python解释器在启动时会自动加载一些模块到内存中，可以使用sys.modules查看

2. 如果还没有找到就从sys.path给出的目录列表中依次寻找my_module.py文件。

**注意：自定义的模块名不应该与系统内置模块重名**

# 编译python文件

为了提高加载模块的速度，python解释器会在 `__pycache__` 目录中下缓存每个模块编译后的版本，格式为：`module.version.pyc`。通常会包含python的版本号。例如，在CPython3.3版本下， `my_module.py`模块会被缓存成 `__pycache__`/`my_module.cpython-33.pyc` 。这种命名规范保证了编译 后的结果多版本共存。

# 包

包就是一个包含有 `__init__.py` 文件的文件夹，所以其实我们创建包的目的就是为了用文件夹将文件/ 模块组织起来

需要强调的是：

1. 在python3中，即使包下没有 __init__.py 文件，import 包仍然不会报错，而在python2中，包 下一定要有该文件，否则import 包报错
2. 创建包的目的不是为了运行，而是被导入使用，记住，包只是模块的一种形式而已，包的本质就是一种模块

## 为何要使用包

**包的本质就是一个文件夹，那么文件夹唯一的功能就是将文件组织起来**

随着功能越写越多，我们无法将所以功能都放到一个文件中，于是我们使用模块去组织功能，而随着模 块越来越多，我们就需要用文件夹将模块文件组织起来，以此来提高程序的结构性和可维护性

## 注意事项

1. 关于包相关的导入语句也分为 import 和 from ... import ... 两种，但是无论哪种，无论在什 么位置，在导入时都必须遵循一个原则：凡是在导入时带点的，点的左边都必须是一个包，否则非 法。可以带有一连串的点，如 item.subitem.subsubitem ,但都必须遵循这个原则。但对于导入 后，在使用时就没有这种限制了，点的左边可以是包,模块，函数，类(它们都可以用点的方式调用 自己的属性)。
2.  import导入文件时，产生名称空间中的名字来源于文件，import 包，产生的名称空间的名字同样 来源于文件，即包下的 __init__.py ，导入包本质就是在导入该文件
3. 包A和包B下有同名模块也不会冲突，如A.a与B.a来自两个命名空间

## 包的使用

示例文件

```python
glance/                         #Top-level package
├── __init__.py                 #Initialize the glance package
├── api                         #Subpackage for api
│ ├── __init__.py
│ ├── policy.py
│ └── versions.py
├── cmd                         #Subpackage for cmd
│ ├── __init__.py
│ └── manage.py
└── db                          #Subpackage for db
├── __init__.py
└── models.py
```

文件内容

```python
#文件内容
#policy.py
def get():
	print('from policy.py')
	
#versions.py
def create_resource(conf):
	print('from version.py: ',conf)
	
#manage.py
def main():
	print('from manage.py')

#models.py
def register_models(engine):
	print('from models.py: ',engine)
```

## 使用import导入包

```python
import glance.db.models
# 在导入glance的时候会执行glance下的__init__.py中的代码

glance.db.models.register_models('mysql')
```

单独导入包名称时不会导入包中所有包含的所有子模块

```python
import glance
glance.cmd.manage.main()
```

解决方法

```python
# glance/__init__.py
from . import cmd

# glance/cmd/__init__.py
from . import manage
```

使用from （具体的路径） import （具体的模块）

需要注意的是from后import导入的模块，必须是明确的一个不能带点，否则会有语法错误，如： `from a import b.c` 是错误语法

```python
from glance.db import models
from glance.db.models import register_models

models.register_models('mysql')
register_models('mysql')
```

`from glance.api import *`

想从包`api`中导入所有，实际上该语句只会导入包`api`下 `__init__.py` 文件中定义的名字，我们可以在 `__init__.py` 这个文件中定义 `__all__`

```python
x = 10

def func():
	print('from api.__init.py')

__all__=['x','func','policy']
```

```python
from glance.api import *

func()
print(x)
policy.get()
```

# 绝对导入和相对导入

- 绝对导入：以glance作为起始
- 相对导入：用.或者..的方式最为起始（只能在一个包中使用，不能用于不同目录内）

绝对导入: 以执行文件的sys.path为起始点开始导入,称之为绝对导入

1. 优点: 执行文件与被导入的模块中都可以使用
2. 缺点: 所有导入都是以sys.path为起始点,导入麻烦

相对导入: 参照当前所在文件的文件夹为起始开始查找,称之为相对导入

1. 符号: .代表当前所在文件的文件加,..代表上一级文件夹,...代表上一级的上一级文件夹
2. 优点: 导入更加简单
3. 缺点: 只能在导入包中的模块时才能使用

注意:

- 相对导入只能用于包内部模块之间的相互导入,导入者与被导入者都必须存在于一个包内
- 试图在顶级包之外使用相对导入是错误的,言外之意,必须在顶级包内使用相对导入,每增加一个.代表 跳到上一级文件夹,而上一级不应该超出顶级包
