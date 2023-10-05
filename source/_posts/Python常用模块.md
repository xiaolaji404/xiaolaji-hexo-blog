---
title: Python常用模块
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python常用模块.jpg
banner_img: /Photo/Blog_Photo/Banner/Python常用模块.jpg
typora-root-url: Python常用模块
date: 2022-10-01 00:02
---

# 序列化模块

将原本的字典、列表等内容转换成一个字符串的过程就叫做序列化

**序列化的目的**

-  以某种存储形式使自定义对象持久化；
- 将对象从一个地方传递到另一个地方。
- 使程序更具维护性

![相互关系](image-20220707204234435.png)

python可序列化的数据类型，序列化出来之后的结果如下

| Python     | JSON   |
| ---------- | ------ |
| dict       | object |
| list,tuple | array  |
| str        | string |
| int,float  | number |
| True       | true   |
| False      | false  |
| None       | null   |

## json模块(很重要的)

Json模块提供了四个功能：dumps、dump、loads、load

```python
import json

dic = {'k1':'v1','k2':'v2','k3':'v3'}
str_dic = json.dumps(dic)#将Python中的类型转换为字符串
# 序列化：将一个字典转换成一个字符串

print(type(str_dic),str_dic)
dic2 = json.loads(str_dic)#字符串转回原本的类型
print(type(dic2),dic2)
# 反序列化：将一个字符串格式的字典转换成一个字典

list_dic = [1,['a','b','c'],3,{'k1':'v1','k2':'v2'}]
str_dic = json.dumps(list_dic)

print(type(str_dic),str_dic)
list_dic2 = json.loads(str_dic)
print(type(list_dic2),list_dic2)
```

json.dumps的参数：

| Skipkeys     | 1,默认值是False，如果dict的keys内的数据不是python的基本类型,2,设置为 False时，就会报TypeError的错误。此时设置成True，则会跳过这类key，3,当 它为True的时候，所有非ASCII码字符显示为\uXXXX序列，只需在dump时将 ensure_ascii设置为False即可，此时存入json的中文即可正常显示。 |
| ------------ | ------------------------------------------------------------ |
| indent       | 是一个非负的整型，如果是0就是顶格分行显示，如果为空就是一行最紧凑显示， 否则会换行且按照indent的数值显示前面的空白分行显示，这样打印出来的json 数据也叫pretty-printed json |
| ensure_ascii | 当它为True的时候，所有非ASCII码字符显示为\uXXXX序列，只需在dump时将 ensure_ascii设置为False即可，此时存入json的中文即可正常显示。 |
| separators   | 分隔符，实际上是(item_separator, dict_separator)的一个元组，默认的就是 (‘,’,’:’)；这表示dictionary内keys之间用“,”隔开，而KEY和value之间用“：”隔开。 |
| sort_keys    | 将数据根据keys的值进行排序                                   |

```python
import json

data = {'name':'马牛逼','sex':'female','age':88}
json_dic2 = json.dumps(data,sort_keys=True,indent=2,separators=
(',',':'),ensure_ascii=False)
print(json_dic2)
```

json.dump和json.load不常用，主要是针对文件操作进行序列化和反序列化

```python
序列化：
import json
v = {'k1':'yh','k2':'小马过河'}
f = open('xiaoma.txt',mode='w+',encoding='utf-8') #文件不存在就会生成
val = json.dump(v,f,ensure_ascii=False)
data=f.read()
print(type(data))
print(val)
f.close()
----------------结果：
<class 'str'>
None
#dump将内容序列化，并写入打开的文件中。

反序列化：
import json
f = open('xiaoma.txt',mode='r',encoding='utf-8')
data = json.load(f)
f.close()
print(data,type(data))
---------------结果:
{'k1': 'yh', 'k2': '小马过河'} <class 'dict'>
```

## pickle模块

| json   | 用于字符串 和 python数据类型间进行转换             |
| ------ | -------------------------------------------------- |
| pickle | 用于python特有的类型 和 python的数据类型间进行转换 |

pickle模块提供了四个功能：dumps、dump(序列化，存）、loads（反序列化，读）、load 

不仅可以序列化字典，列表...可以把python中任意的数据类型序列化

json模块和picle模块都有 dumps、dump、loads、load四种方法，而且用法一样。 

不同的是json模块序列化出来的是通用格式，其它编程语言都认识，就是普通的字符串， 

而picle模块序列化出来的只有python可以认识，其他编程语言不认识的，表现为乱码 

不过picle可以序列化函数，但是其他文件想用该函数，在该文件中需要有该文件的定义（定义和参数必 须相同，内容可以不同）

```python
import pickle
dic = {'k1':'v1','k2':'v2','k3':'v3'}
str_dic = pickle.dumps(dic)
print(str_dic)

dic2 = pickle.loads(str_dic)
print(dic2)

import time
struct_time = time.localtime(1000000000)
print(struct_time)
f = open('pickle_file','wb')
pickle.dump(struct_time,f)
f.close()

f = open('pickle_file','rb')
struct_time2 = pickle.load(f)
print(struct_time2.tm_year)
```

## shelve模块

shelve也是python提供给我们的序列化工具，比pickle用起来更简单一些。 

shelve只提供给我们一个open方法，是用key来访问的，使用起来和字典类似。

参考博客

https://www.cnblogs.com/sui776265233/p/9225164.html

```python
import shelve
f = shelve.open('shelve_file')
f['key'] = {'int':10,'str':'hello','float':0.123}
f.close()

f1 = shelve.open('shelve_file')
ret = f1['key']
f1.close()
print(ret)
```

这个模块有个限制，它不支持多个应用同一时间往同一个DB进行写操作。所以当我们知道我们的应用如 果只进行读操作，我们可以让shelve通过只读方式打开DB

```python
import shelve
f1 = shelve.open('shelve_file',flag='r')
ret = f1['key']
f1.close()
print(ret)
```

由于shelve在默认情况下是不会记录待持久化对象的任何修改的，所以我们在shelve.open()时候需要修 改默认参数，否则对象的修改不会保存。

```python
import shelve
f1 = shelve.open('shelve_file')
print(f1['key'])
f1['key']['k1'] = 'v1'
print(f1['key'])
f1.close()

f2 = shelve.open('shelve_file',writeback=True) #开启后才能写生效

f2['key']['k1'] = 'hello'
print(f2['key'])
f2.close()
```

使用shelve模块实现简单的数据库

```python
# 简单的数据库

import sys,shelve

def print_help():
'存储（增加）、查找、更新（修改）、循环打印、删除、退出、帮助'
	print('The available commons are: ')
	print('store : Stores information about a person')
	print('lookup : Looks up a person from ID numbers')
	print("update : Update a person's information from ID number")
	print('print_all: Print all informations')
	print("delete : Delete a person's information from ID number")
	print('quit : Save changes and exit')
	print('? : Print this message')
	
def store_people(db):
	pid = input('Please enter a unique ID number: ')
	person = {}
	person['name'] = input('Please enter the name: ')
	person['age'] = input('Please enter the age: ')
	person['phone'] = input('Please enter the phone: ')
	db[pid] = person
	print("Store information: pid is %s, information is %s" % (pid, person))
	
def lookup_people(db):
	pid = input('Please enter the number: ')
	field = input('What would you like to know? (name, age, phone) ')
	if pid in db.keys():
		value = db[pid][field]
		print("Pid %s's %s is %s" % (pid, field, value))
	else:
		print('Not found this number')
		
def update_people(db):
	pid = input('Please enter the number: ')
	field = input('What would you like to update? (name, age, phone) ')
	newvalue = input('Enter the new information: ')
	if pid in db.keys():
		value = db[pid]
		value[field] = newvalue
		print("Pid %s's %s update information is %s" % (pid, field,newvalue))
	else:
		print("Not found this number, can't update")
		
def delete_people(db):
	pid = input('Please enter the number: ')
	if pid in db.keys():
		del db[pid]
		print("pid %s's information delete done" % pid)
	else:
		print( "Not found this number, can't delete")
		
def print_all_people(db):
	print( 'All information are: ')
	for key, value in db.items():
		print(key, value)
		
def enter_cmd():
	cmd = input('Please enter the cmd(? for help): ')
	cmd = cmd.strip().lower()
	return cmd
	
def main():
	database = shelve.open('database201803.dat', writeback=True)
	try:
		while True:
			cmd = enter_cmd()
			if cmd == 'store':
				store_people(database)
			elif cmd == 'lookup':
				lookup_people(database)
			elif cmd == 'update':
				update_people(database)
			elif cmd == 'print_all':
				print_all_people(database)
			elif cmd == 'delete':
				delete_people(database)
			elif cmd == '?':
				print_help()
			elif cmd == 'quit':
				return
	finally:
		database.close()
if __name__ == '__main__':
	main()
```

## hashlib模块（用于加密提供了大量的加密算法）

Python的`hashlib`提供了常见的摘要算法，如MD5，SHA1等等。

什么是摘要算法呢？摘要算法又称哈希算法、散列算法。它通过一个函数，把任意长度的数据转换为一 个长度固定的数据串（通常用16进制的字符串表示）。

摘要算法就是通过摘要函数f()对任意长度的数据data计算出固定长度的摘要digest，目的是为了发现原 始数据是否被人篡改过。

摘要算法之所以能指出数据是否被篡改过，就是因为摘要函数是一个单向函数，计算f(data)很容易，但 通过digest反推data却非常困难。而且，对原始数据做一个bit的修改，都会导致计算出的摘要完全不同。

```python
import hashlib

md5 = hashlib.md5()
md5.update('how to use md5 in python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
```

MD5是最常见的摘要算法，速度很快，生成结果是固定的128 bit字节，通常用一个32位的16进制字符 串表示。另一种常见的摘要算法是SHA1，调用SHA1和调用MD5完全类似

```python
import hashlib

sha1 = hashlib.sha1()
sha1.update('how to use md5 '.encode('utf-8'))
sha1.update('in python hashlib?'.encode('utf-8'))
print(sha1.hexdigest())
```

利用`hashlib`将我们的注册登录模块保存的密码进行加密

```python
#注册部分修改
	with open("db.txt", "a", encoding="utf-8") as write_file:
		md5=hashlib.md5()
		md5.update(user_name.encode('utf-8'))
		md5.update(user_password.encode('utf-8'))
		user_password_md5=md5.hexdigest()
		user_info = user_name + ":" + user_password_md5 + "\n
		write_file.write(user_info)
		print("注册成功")
        
#登录部分修改
	while True:
		# 确保用户密码正确
		user_password = input("请输入登录密码；\n")
		flag2 = False
		for dict4 in list1:
			md5 =hashlib.md5()
			md5.update(user_name.encode('utf-8'))
			md5.update(user_password.encode('utf-8'))
			user_password_md5=md5.hexdigest()
			if dict4["username"] == user_name and dict4["password"] ==user_password_md5:
				flag2 = True

```

## 摘要算法应用

任何允许用户登录的网站都会存储用户登录的用户名和口令。如何存储用户名和口令呢？方法是存到数据库表中

```python
name    | password
--------+----------
michael | 123456
bob     | abc999
alice   | alice2008
```

如果使用md5来将保护密码那么就是这样

```
username | password
---------+---------------------------------
michael  | e10adc3949ba59abbe56e057f20f883e
bob      | 878ef96e86145580c38c87f0410ad153
alice    | 99b1c2188db85afee403b1536010c2c9
```

有很多md5撞库工具，可以轻松的将简单密码给碰撞出来

所以，要确保存储的用户口令不是那些已经被计算出来的常用口令的MD5，这一方法通过对原始口令加 一个复杂字符串来实现，俗称“加盐

经过Salt处理的MD5口令，只要Salt不被黑客知道，即使用户输入简单口令，也很难通过MD5反推明文口令

但是如果有两个用户都使用了相同的简单口令比如123456，在数据库中，将存储两条相同的MD5值， 这说明这两个用户的口令是一样的。

如果假定用户无法修改登录名，就可以通过把登录名作为Salt的一部分来计算MD5，从而实现相同口令 的用户也存储不同的MD5。

# configparser模块（专门对配置文件的修改）

该模块适用于配置文件的格式与windows ini文件类似，可以包含一个或多个节（section），每个节可 以有多个参数（键=值）。

常见的文档格式

```python
[default]
ServerAliveInterval = 45
Compression = yes
CompressionLevel = 9
ForwardX11 = yes

[bitbucket.org]
User = hg

[topsecret.server.com]
Port = 50022
ForwardX11 = no
```

使用python生成一个这样的文件

```python
import configparser

conf = configparser.ConfigParser()

conf['default'] = {'ServerAliveInterval':'45',
				 'Compression':'yes',
				 'CompressionLevel':'9',
				 'ForwardX11':'yes'
				 }
conf['bitbucket.org'] = {'User':'hg'}
conf['topsecret.server.com'] = {'Port':'50022',
							 'ForwardX11':'no'
                               }
with open('config','w') as config:
	conf.write(config)
```

查找配置文件中的所有section和option

```python
import configparser

conf = configparser.ConfigParser()
conf.read('config')

secs = conf.sections()
print(secs)
for sec in secs:
	print(sec,conf.options(sec))
```

查找配置文件中option的值

```python
import configparser

conf = configparser.ConfigParser()
conf.read('config')

secs = conf.sections()
print(secs)
for sec in secs:
	print(sec,conf.options(sec))

serveraliveinterval=conf.get('default','serveraliveinterval')
print(serveraliveinterval)
user=conf.get('bitbucket.org','user')
print(user)
```

修改配置文件

```python
import configparser

conf = configparser.ConfigParser()
conf.read('config')

# 使用has_section来判断是否有这个section配置项，option可以使用has_option方法来判断是否存在
if not conf.has_section('ABC'):
	# 如果没有则添加这个section
	conf.add_section("ABC")
	# 并在该section下添加一个名叫abc的option项
	conf.set("ABC", "abc", "123")
conf.set('topsecret.server.com','ip','192.168.1.1')
	# 将这些修改写入到配置文件中
with open('config', 'w+') as f:
	conf.write(f)
```

删除某些section和option

```python
import configparser

conf = configparser.ConfigParser()
conf.read('config')
# 删除ABC section，包含下面所有的option都会一并删除
conf.remove_section('ABC')
# 删除topsecret.server.com下的ip ，其他option不会受影响
conf.remove_option('topsecret.server.com', 'ip')

with open('config', 'w+') as f:
conf.write(f)
```

查找配置文件中option的值

```python
import configparser

conf = configparser.ConfigParser()
conf.read('config')

secs = conf.sections()
print(secs)
for sec in secs:
	print(sec,conf.options(sec))
	
serveraliveinterval=conf.get('default','serveraliveinterval')
print(serveraliveinterval)
user=conf.get('bitbucket.org','user')
print(user)
```

修改配置文件

```python
import configparser

conf = configparser.ConfigParser()
conf.read('config')

# 使用has_section来判断是否有这个section配置项，option可以使用has_option方法来判断是否存在

if not conf.has_section('ABC'):
	# 如果没有则添加这个section
	conf.add_section("ABC")
	# 并在该section下添加一个名叫abc的option项
	conf.set("ABC", "abc", "123")
conf.set('topsecret.server.com','ip','192.168.1.1')
	# 将这些修改写入到配置文件中
with open('config', 'w+') as f:
	conf.write(f)
```

删除某些section和option

```python
import configparser

conf = configparser.ConfigParser()
conf.read('config')
# 删除ABC section，包含下面所有的option都会一并删除
conf.remove_section('ABC')
# 删除topsecret.server.com下的ip ，其他option不会受影响
conf.remove_option('topsecret.server.com', 'ip')

with open('config', 'w+') as f:
	conf.write(f)
```

# logging模块（记录程序运行的日志模块）

参考博客：

https://blog.csdn.net/pansaky/article/details/90710751

## 函数式简单配置

```python
import logging
#日志的五个等级
logging.debug('debug message')
logging.info('info message')
logging.warning('warning message')
logging.error('error message')
logging.critical('critical message')
```

默认情况下Python的logging模块将日志打印到了标准输出中，且只显示了大于等于WARNING级别的日 志，这说明默认的日志级别设置为WARNING（日志级别等级CRITICAL > ERROR > WARNING > INFO > DEBUG），默认的日志格式为日志级别：Logger名称：用户输出消息。

```python
import logging

logging.basicConfig(level=logging.DEBUG,
				  format='%(asctime)s %(filename)s[line:%(lineno)d] %
(levelname)s %(message)s',
					datefmt='%a, %d %b %Y %H:%M:%S',
					filename='test.log',
					filemode='w')
					
logging.debug('debug message')
logging.info('info message')
logging.warning('warning message')
logging.error('error message')
logging.critical('critical message')
```

参数解释

- ogging.basicConfig()函数中可通过具体参数来更改logging模块默认行为，可用参数有：

- filename：用指定的文件名创建FiledHandler，这样日志会被存储在指定的文件中。

- filemode：文件打开方式，在指定了filename时使用这个参数，默认值为“a”还可指定为“w”。

- format：指定handler使用的日志显示格式。

- datefmt：指定日期时间格式。

- level：设置rootlogger（后边会讲解具体概念）的日志级别

- stream：用指定的stream创建StreamHandler。可以指定输出到sys.stderr,sys.stdout或者文件 (f=open- (‘test.log’,’w’))，默认为sys.stderr。若同时列出了filename和stream两个参数，则 stream参数会被忽略。

- format参数中可能用到的格式化串

  %(name)s Logger的名字

  %(levelno)s 数字形式的日志级别

  %(levelname)s 文本形式的日志级别

  %(pathname)s 调用日志输出函数的模块的完整路径名，可能没有

  %(filename)s 调用日志输出函数的模块的文件名

  %(module)s 调用日志输出函数的模块名

  %(funcName)s 调用日志输出函数的函数名

  %(lineno)d 调用日志输出函数的语句所在的代码行

  %(created)f 当前时间，用UNIX标准的表示时间的浮 点数表示

  %(relativeCreated)d 输出日志信息时的，自Logger创建以 来的毫秒数

  %(asctime)s 字符串形式的当前时间。默认格式是 “2003-07-08 16:49:45,896”。逗号后面的是毫秒

  %(thread)d 线程ID。可能没有

  %(threadName)s 线程名。可能没有

  %(process)d 进程ID。可能没有

  %(message)s用户输出的消息

# logger对象配置（也与日志有关）

```python
import logging

logger = logging.getLogger()

# 创建一个handler，用于写入日志文件
fh = logging.FileHandler('test.log',encoding='utf-8')
# 再创建一个handler，用于输出到控制台
ch = logging.StreamHandler()

formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

fh.setFormatter(formatter)

ch.setFormatter(formatter)

logger.addHandler(fh) #logger对象可以添加多个fh和ch对象
logger.addHandler(ch)
logger.debug('logger debug message')
logger.info('logger info message')
logger.warning('logger warning message')
logger.error('logger error message')
logger.critical('logger critical message')
```

logging库提供了多个组件：Logger、Handler、Filter、Formatter。Logger对象提供应用程序可直接使用的接口，Handler发送日志到适当的目的地，Filter提供了过滤日志信息的方法，Formatter指定日志显示格式。

## logger的配置文件

```python
"""
logging配置
"""

import os
import logging.config

# 定义三种日志输出格式 开始

standard_format = '[%(asctime)s][%(threadName)s:%(thread)d][task_id:%(name)s][%(filename)s:%(lineno)d]' \
                  '[%(levelname)s][%(message)s]' #其中name为getlogger指定的名字
                  
simple_format = '[%(levelname)s][%(asctime)s][%(filename)s:%(lineno)d]%(message)s'

id_simple_format = '[%(levelname)s][%(asctime)s] %(message)s'

# 定义日志输出格式 结束

logfile_dir = os.path.dirname(os.path.abspath(__file__)) # log文件的目录

logfile_name = 'all2.log' # log文件名

# 如果不存在定义的日志目录就创建一个
if not os.path.isdir(logfile_dir):
	os.mkdir(logfile_dir)
	
# log文件的全路径
logfile_path = os.path.join(logfile_dir, logfile_name)

# log配置字典
LOGGING_DIC = {
	'version': 1,
	'disable_existing_loggers': False,
	'formatters': {
		'standard': {
			'format': standard_format
		},
		'simple': {
			'format': simple_format
		},
	},
	'filters': {},
	'handlers': {
		#打印到终端的日志
		'console': {
			'level': 'DEBUG',
			'class': 'logging.StreamHandler', # 打印到屏幕
			'formatter': 'simple'
		},
		#打印到文件的日志,收集info及以上的日志
		'default': {
			'level': 'DEBUG',
			'class': 'logging.handlers.RotatingFileHandler', # 保存到文件
			'formatter': 'standard',
			'filename': logfile_path, # 日志文件
			'maxBytes': 1024*1024*5, # 日志大小 5M
			'backupCount': 5,
			'encoding': 'utf-8', # 日志文件的编码，再也不用担心中文log乱码了
		},
	},
	'loggers': {
		#logging.getLogger(__name__)拿到的logger配置
		'': {
			'handlers': ['default', 'console'], # 这里把上面定义的两个handler都加上，即log数据既写入文件又打印到屏幕
			'level': 'DEBUG',
			'propagate': True, # 向上（更高level的logger）传递
		},
	},
}
def load_my_logging_cfg():
    logging.config.dictConfig(LOGGING_DIC) # 导入上面定义的logging配置
	logger = logging.getLogger(__name__) # 生成一个log实例
	logger.info('It works!') # 记录该文件的运行状态
    
if __name__ == '__main__':
	load_my_logging_cfg()
```

```python
注意：


#1、有了上述方式我们的好处是：所有与logging模块有关的配置都写到字典中就可以了，更加清晰，方便管理


#2、我们需要解决的问题是：
	1、从字典加载配置：logging.config.dictConfig(settings.LOGGING_DIC)
	
	2、拿到logger对象来产生日志
	logger对象都是配置到字典的loggers 键对应的子字典中的
	按照我们对logging模块的理解，要想获取某个东西都是通过名字，也就是key来获取的
	于是我们要获取不同的logger对象就是
	logger=logging.getLogger('loggers子字典的key名')
	
	
但问题是：如果我们想要不同logger名的logger对象都共用一段配置，那么肯定不能在loggers子字典中定义n个key
'loggers': {
		'l1': {
			'handlers': ['default', 'console'], #
			'level': 'DEBUG',
			'propagate': True, # 向上（更高level的logger）传递
		},
		'l2: {
			'handlers': ['default', 'console' ],
			'level': 'DEBUG',
			'propagate': False, # 向上（更高level的logger）传递
		},
		'l3': {
			'handlers': ['default', 'console'], #
			'level': 'DEBUG',
			'propagate': True, # 向上（更高level的logger）传递
		},
}


#我们的解决方式是，定义一个空的key
	'loggers': {
		'': {
			'handlers': ['default', 'console'],
			'level': 'DEBUG',
			'propagate': True,
		},
}

这样我们再取logger对象时
logging.getLogger(__name__)，不同的文件__name__不同，这保证了打印日志时标识信息不同，但是拿着该名字去loggers里找key名时却发现找不到，于是默认使用key=''的配置
```

# collections模块(用来提供额外的数据结构)

在内置数据类型（dict、list、set、tuple）的基础上，collections模块还提供了几个额外的数据类型： Counter、deque、defaultdict、namedtuple和OrderedDict等。

1. namedtuple: 生成可以使用名字来访问元素内容的tuple
2. deque: 双端队列，可以快速的从另外一侧追加和推出对象
3. Counter: 计数器，主要用来计数
4. defaultdict: 带有默认值的字典

## namedtuple（用于表示二位坐标）

```python
from collections import namedtuple
point = namedtuple('point',['x','y'])
p = point(1,2)
print(p.x)
```

一个点的二维坐标就可以表示成,但是，看到(1, 2)，很难看出这个tuple是用来表示一个坐标的。 这时，namedtuple就派上了用场

## deque（双向链表，可高效插入删除操作，适合队列和栈）

```python
from collections import deque

q = deque(['a','b','c'])
q.append('x')
q.appendleft('y')

print(q)
```

deque除了实现list的append()和pop()外，还支持appendleft()和popleft()，这样就可以非常高效地往头 部添加或删除元素。

## defaultdict

有如下值列表 [11,22,33,44,55,66,77,88,99,90...]，将所有大于 66 的值保存至字典的第一个key中，将小 于 66 的值保存至第二个key的值中。

即： {'k1': 大于66 , 'k2': 小于66}

```python
# 不用defaultdict的方式
li = [11,22,33,44,55,77,88,99,90]

result = {}
for row in li:
	if row < 66:
		if 'key1' not in result:
			result['key1']=[]
		result['key1'].append(row)
	else:
		if 'key2' not in result:
			result['key2']=[]
		result['key2'].append(row)
print(result)


# 用defaultdict的方式
from collections import defaultdict

li = [11,22,33,44,55,77,88,99,90]
result=defaultdict(list)
print(result)
for row in li:
	if row > 66:
		result['key1'].append(row)
	else:
		result['key2'].append(row)
		
print(result)
```

## counter

Counter类的目的是用来跟踪值出现的次数。它是一个无序的容器类型，以字典的键值对形式存储，其中元素作为key，其计数作为value。

```python
from collections import Counter

c = Counter('qazxswqazxswqazxswsxaqwsxaqws')
print(c)
```

# 时间有关的模块

## time模块

常用方法

- time.sleep(secs)

  	(线程)推迟指定的时间运行。单位为秒。

- time.time()

  	获取当前时间戳

表示时间的三种方式

在Python中，通常有这三种方式来表示时间：时间戳、元组(struct_time)、格式化的时间字符串：

1. 时间戳(timestamp) ：通常来说，时间戳表示的是从1970年1月1日00:00:00开始按秒计算的偏移 量。我们运行“type(time.time())”，返回的是float类型。

2. 格式化的时间字符串(Format String)： ‘1999-12-06’

   

   

| %y   | 两位数的年份表示（00-99）                 |
| ---- | ----------------------------------------- |
| %Y   | 四位数的年份表示（000-9999）              |
| %m   | 月份（01-12）                             |
| %d   | 月内中的一天（0-31）                      |
| %H   | 24小时制小时数（0-23）                    |
| %I   | 12小时制小时数（01-12）                   |
| %M   | 分钟数（00=59）                           |
| %S   | 秒（00-59）                               |
| %a   | 本地简化星期名称                          |
| %A   | 本地完整星期名称                          |
| %b   | 本地简化的月份名称                        |
| %B   | 本地完整的月份名称                        |
| %c   | 本地相应的日期表示和时间表示              |
| %j   | 年内的一天（001-366）                     |
| %p   | 本地A.M.或P.M.的等价符                    |
| %U   | 一年中的星期数（00-53）星期天为星期的开始 |
| %w   | 星期（0-6），星期天为星期的开始           |
| %W   | 一年中的星期数（00-53）星期一为星期的开始 |
| %x   | 本地相应的日期表示                        |
| %X   | 本地相应的时间表示                        |
| %Z   | 当前时区的名称                            |
| %%   | %号本身                                   |

元组(struct_time) ：struct_time元组共有9个元素共九个元素:(年，月，日，时，分，秒，一年中 第几周，一年中第几天等）

| 索引（Index） | 属性（Attribute）         | 值（Values）       |
| ------------- | ------------------------- | ------------------ |
| 0             | tm_year（年）             | 比如2011           |
| 1             | tm_mon（月）              | 1月12日            |
| 2             | tm_mday（日）             | 1月31日            |
| 3             | tm_hour（时）             | 0 - 23             |
| 4             | tm_min（分）              | 0 - 59             |
| 5             | tm_sec（秒）              | 0 - 60             |
| 6             | tm_wday（weekday）        | 0 - 6（0表示周一） |
| 7             | tm_yday（一年中的第几天） | 1 - 366            |
| 8             | tm_isdst（是否是夏令时）  | 默认为0            |

```
import time

# 第一种时间格式，时间戳的形式
print(time.time())

# 第二种时间格式，格式化的时间
print(time.strftime('%Y-%m-%d %X'))
print(time.strftime('%Y-%m-%d %H-%M-%S'))

# 第三种时间格式，结构化的时间，是一个元组
print(time.localtime())
```

小结：时间戳是计算机能够识别的时间；时间字符串是人能够看懂的时间；元组则是用来操作时间的

**几种格式之间的转换**

![转换示意图](image-20220708192657540.png)

```python
import time
# 格式化时间 ----> 结构化时间
ft = time.strftime('%Y/%m/%d %H:%M:%S')
print('ft',ft)
st = time.strptime(ft,'%Y/%m/%d %H:%M:%S')
print('st',st)
# 结构化时间 ---> 时间戳
t = time.mktime(st)
print('t',t)

# 时间戳 ----> 结构化时间
t = time.time()
st = time.localtime(t)
print('st',st)
# 结构化时间 ---> 格式化时间
ft = time.strftime('%Y/%m/%d %H:%M:%S',st)
print('ft',ft)
```

![转换示意图](image-20220708192814418.png)

```
import time

#结构化时间 --> %a %b %d %H:%M:%S %Y串
#time.asctime(结构化时间) 如果不传参数，直接返回当前时间的格式化串
print(time.asctime(time.localtime(1550312090.4021888)))

#时间戳 --> %a %d %d %H:%M:%S %Y串
#time.ctime(时间戳) 如果不传参数，直接返回当前时间的格式化串
print(time.ctime(1550312090.4021888))
```

计算时间差

```python
import time

start_time=time.mktime(time.strptime('2017-09-11 08:30:00','%Y-%m-%d
%H:%M:%S'))
end_time=time.mktime(time.strptime('2019-09-12 11:00:50','%Y-%m-%d
%H:%M:%S'))
dif_time=end_time-start_time
struct_time=time.gmtime(dif_time)
print('过去了%d年%d月%d天%d小时%d分钟%d秒'%(struct_time.tm_year-1970,struct_time.tm_mon-1,
struct_time.tm_mday-1,struct_time.tm_hour,

struct_time.tm_min,struct_time.tm_sec))
```

显示进度条

```python
import time

for i in range(0,101,2):
	time.sleep(0.1)
	char_num = i//2
	per_str = '\r%s%% : %s\n' % (i, '*' * char_num) \
		if i == 100 else '\r%s%% : %s' % (i,'*'*char_num)
	print(per_str,end='', flush=True)
```

这是换一种方式显示

```
import time
print("游戏开始加载")
for i in range(0, 101, 2):
	time.sleep(0.1)
	char_num = i // 2
	per_str = '\r[%s' % ('#' * char_num)
	
	print(per_str+"%s" % "_" *(50 - char_num), "%s%%]" % i, end='')
print("")
```

## datatime模块

```python
# datatime模块
import datetime
now_time = datetime.datetime.now() # 现在的时间
# 只能调整的字段：weeks days hours minutes seconds
print(datetime.datetime.now() + datetime.timedelta(weeks=3)) # 三周后
print(datetime.datetime.now() + datetime.timedelta(weeks=-3)) # 三周前
print(datetime.datetime.now() + datetime.timedelta(days=-3)) # 三天前
print(datetime.datetime.now() + datetime.timedelta(days=3)) # 三天后
print(datetime.datetime.now() + datetime.timedelta(hours=5)) # 5小时后
print(datetime.datetime.now() + datetime.timedelta(hours=-5)) # 5小时前
print(datetime.datetime.now() + datetime.timedelta(minutes=-15)) # 15分钟前
print(datetime.datetime.now() + datetime.timedelta(minutes=15)) # 15分钟后
print(datetime.datetime.now() + datetime.timedelta(seconds=-70)) # 70秒前
print(datetime.datetime.now() + datetime.timedelta(seconds=70)) # 70秒后
current_time = datetime.datetime.now()
# 可直接调整到指定的 年 月 日 时 分 秒 等

print(current_time.replace(year=1977)) # 直接调整到1977年
print(current_time.replace(month=1)) # 直接调整到1月份
print(current_time.replace(year=1989,month=4,day=25)) # 1989-04-25
18:49:05.898601

# 将时间戳转化成时间
print(datetime.date.fromtimestamp(1232132131)) # 2009-01-17
```

## random模块

用来生成随机数模块

```python
import random

print(random.random()) # 大于0且小于1之间的小数
print(random.uniform(1,3)) # 大于1小于3的小数

print(random.randint(1,5)) # 大于等于1且小于等于5之间的整数
print(random.randrange(1,10,2)) # 大于等于1且小于10之间的奇数

ret = random.choice([1,'23',[4,5]]) # 1或者23或者[4,5]
print(ret)

a,b = random.sample([1,'23',[4,5]],2) # 列表元素任意2个组合 sample(seq, n) 从
序列seq中选择n个随机且独立的元素
print(a,b)
# 发牌

item = [1,3,5,7,9]
random.shuffle(item) # 打乱次序
print(item)
```

生成随机验证码

```python
import random

def v_code():

	code = ''
	for i in range(5):
	
		num=random.randint(0,9)
		alf=chr(random.randint(65,90)) #chr() 用一个范围在 range（256）内的（就是0～255）整数作参数，返回一个对应的字符。返回值是当前整数对应的 ASCII 字符。
		add=random.choice([num,alf])
		code="".join([code,str(add)])
		
	return code
	
print(v_code())
```

注册登录案例添加验证码

```py
def get_code():
    code = ''
    for i in range(5):
        num = random.randint(0, 9)
        alf = chr(random.randint(65, 90))  # 用ASCII码来返回大小写的字母
        add = random.choice([num, alf])
        code = ''.join([code, str(add)])
    # print(code)
    return code
```

斗地主发牌

```python
def fight_whit_the_landowner():  # 斗地主游戏代码
    print("斗地主游戏即将开始")
    time_line()
    ci_shu = 0
    farmer1 = []
    farmer2 = []
    landowner = []
    card = []
    for i in range(1, 14):
        if i == 11:
            i = 'J'
        elif i == 12:
            i = 'Q'
        elif i == 13:
            i = 'K'
        elif i == 1:
            i = 'A'
    for i in range(1, 14):
        card.append("♠" + str(i))
        card.append("♣" + str(i))
        card.append("♦" + str(i))
        card.append("♥" + str(i))
    card.append('大王')
    card.append('小王')
    for i in range(1, 10):
        random.shuffle(card)

    while ci_shu < 51:
        farmer1.append(card[1])
        farmer2.append(card[ci_shu + 2])
        landowner.append(card[ci_shu + 3])
        ci_shu += 3
    print('''底牌：%s''' % card[52:55])
    landowner.append(card[52:55])
    print('''一号农民的牌：%s''' % farmer1)
    print('''二号农民的牌：%s''' % farmer2)
    print('''地主的牌：%s''' % landowner)
    choice = input("输入again重新来一局，输入quit退出")
    if choice == 'again':
        fight_whit_the_landowner()
    elif choice == 'quit':
        return
    else:
        return
        
fight_with_landowner()
```

# OS模块

os模块是与操作系统交互的一个接口

当前执行这个python文件的工作目录相关的工作路径

| os.getcwd()         | 获取当前工作目录，即当前python脚本工作的目录路径 |
| ------------------- | ------------------------------------------------ |
| os.chdir("dirname") | 改变当前脚本工作目录；相当于shell下cd            |
| os.curdir           | 返回当前目录: ('.')                              |
| os.pardir           | 获取当前目录的父目录字符串名：('..')             |

文件夹相关

| os.makedirs('dirname1/dirname2') | 可生成多层递归目录                                           |
| -------------------------------- | ------------------------------------------------------------ |
| os.removedirs('dirname1')        | 若目录为空，则删除，并递归到上一级目录，如若也为 空，则删除，依此类推 |
| os.mkdir('dirname')              | 生成单级目录；相当于shell中mkdir dirname                     |
| os.rmdir('dirname')              | 删除单级空目录，若目录不为空则无法删除，报错；相 当于shell中rmdir dirname |
| os.listdir('dirname')            | 列出指定目录下的所有文件和子目录，包括隐藏文件， 并以列表方式打印 |

文件相关

| os.remove()                    | 删除一个文件      |
| ------------------------------ | ----------------- |
| os.rename("oldname","newname") | 重命名文件/目录   |
| os.stat('path/filename')       | 获取文件/目录信息 |

操作系统差异相关

| os.sep     | 输出操作系统特定的路径分隔符，win下为"\",Linux下为"/"   |
| ---------- | ------------------------------------------------------- |
| os.sep     | 输出当前平台使用的行终止符，win下为"\t\n",Linux下为"\n" |
| os.pathsep | 输出当前平台使用的行终止符，win下为"\t\n",Linux下为"\n" |
| os.name    | 输出当前平台使用的行终止符，win下为"\t\n",Linux下为"\n" |

执行系统命令相关

| 执行系统命令相关 | os.environ                  |
| ---------------- | --------------------------- |
| 执行系统命令相关 | 运行shell命令，获取执行结果 |
| os.environ       | 获取系统环境变量            |

path系列，和路径相关

| os.path.abspath(path)               | 返回path规范化的绝对路径                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| os.path.split(path)                 | 将path分割成目录和文件名二元组返回                           |
| os.path.dirname(path)               | 返回path的目录。其实就是os.path.split(path)的第一个元素      |
| os.path.basename(path)              | 返回path最后的文件名。如何path以／或\结尾，那么就会返回空 值，即os.path.split(path)的第二个元素。 |
| os.path.exists(path)                | 如果path存在，返回True；如果path不存在，返回False            |
| os.path.isabs(path)                 | 如果path是绝对路径，返回True                                 |
| os.path.isfile(path)                | 如果path是一个存在的文件，返回True。否则返回False            |
| os.path.isdir(path)                 | 如果path是一个存在的目录，则返回True。否则返回False          |
| os.path.join(path1[, path2[, ...]]) | 将多个路径组合后返回，第一个绝对路径之前的参数将被忽略       |
| os.path.getatime(path)              | 返回path所指向的文件或者目录的最后访问时间                   |
| os.path.getmtime(path)              | 返回path所指向的文件或者目录的最后修改时间                   |
| os.path.getsize(path)               | 返回path的大小                                               |

```python
import os

print(os.stat('.\config')) # 当前目录下的config文件的信息

# 运行结果
# os.stat_result(st_mode=33206, st_ino=2814749767208887, st_dev=1788857329,
st_nlink=1, st_uid=0, st_gid=0, st_size=185, st_atime=1550285376,
st_mtime=1550285376, st_ctime=1550285376)
```



| st_mode  | inode 保护模式                                               |
| -------- | ------------------------------------------------------------ |
| st_ino   | inode 节点号                                                 |
| st_dev   | inode 驻留的设备                                             |
| st_nlink | inode 的链接数                                               |
| st_uid   | 所有者的用户ID                                               |
| st_gid   | 所有者的组ID                                                 |
| st_size  | 普通文件以字节为单位的大小；包含等待某些特殊文件的数据       |
| st_atime | 上次访问的时间                                               |
| st_mtime | 最后一次修改的时间                                           |
| st_ctime | 由操作系统报告的"ctime"。在某些系统上（如Unix）是最新的元数据更改的时间， 在其它系统上（如Windows）是创建时间（详细信息参见平台的文档） |

# sys模块

sys模块是与python解释器交互的一个接口

| sys.argv     | 命令行参数List，第一个元素是程序本身路径，可以接收一些执行程序时传递的 参数 |
| ------------ | ------------------------------------------------------------ |
| sys.exit(n)  | 退出程序，正常退出时exit(0),错误退出sys.exit(1)              |
| sys.version  | 获取Python解释程序的版本信息                                 |
| sys.path     | 返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值       |
| sys.platform | 返回操作系统平台名称                                         |

# re模块(用于网络爬虫)

## 正则表达式

正则就是用一些具有特殊含义的符号组合到一起（称为正则表达式）来描述字符或者字符串的方法。或 者说：正则就是用来描述一类事物的规则。（在Python中）它内嵌在Python中，并通过 re 模块实现。 正则表达式模式被编译成一系列的字节码，然后由用 C 编写的匹配引擎执行

| 元字符 | 匹配内容                                                     |
| :----- | ------------------------------------------------------------ |
| \w     | 匹配字母（包含中文）或数字或下划线                           |
| \W     | 匹配非字母（包含中文）或数字或下划线                         |
| \s     | 匹配任意的空白符                                             |
| \S     | 匹配任意非空白符                                             |
| \d     | 匹配数字                                                     |
| \D     | 匹配非数字                                                   |
| \A     | 从字符串开头匹配                                             |
| \z     | 匹配字符串的结束，如果是换行，只匹配到换行前的结果           |
| \n     | 匹配一个换行符                                               |
| \t     | 匹配一个制表符                                               |
| ^      | 匹配字符串的开始                                             |
| $      | 匹配字符串的结尾                                             |
| .      | 匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任 意字符。 |
| [...]  | 匹配字符组中的字符                                           |
| [^...] | 匹配除了字符组中的字符的所有字符                             |
| *      | 匹配0个或者多个左边的字符。                                  |
| +      | 匹配一个或者多个左边的字符。                                 |
| ?      | 匹配0个或者1个左边的字符，非贪婪方式。                       |
| {n}    | 精准匹配n个前面的表达式。                                    |
| {n,m}  | 匹配n到m次由前面的正则表达式定义的片段，贪婪方式             |
| ()     | 匹配括号内的表达式，也表示一个组                             |

## 单字符匹配

```python
import re

print(re.findall('\w','上大人123asdfg%^&*(_ \t \n)'))
print(re.findall('\W','上大人123asdfg%^&*(_ \t \n)'))

print(re.findall('\s','上大人123asdfg%^&*(_ \t \n)'))
print(re.findall('\S','上大人123asdfg%^&*(_ \t \n)'))

print(re.findall('\d','上大人123asdfg%^&*(_ \t \n)'))
print(re.findall('\D','上大人123asdfg%^&*(_ \t \n)'))

print(re.findall('\A上大','上大人123asdfg%^&*(_ \t \n)'))
print(re.findall('^上大','上大人123asdfg%^&*(_ \t \n)'))

print(re.findall('666\z','上大人123asdfg%^&*(_ \t \n)666'))
print(re.findall('666\Z','上大人123asdfg%^&*(_ \t \n)666'))
print(re.findall('666$','上大人123asdfg%^&*(_ \t \n)666'))

print(re.findall('\n','上大人123asdfg%^&*(_ \t \n)'))
print(re.findall('\t','上大人123asdfg%^&*(_ \t \n)'))
```

## 重复匹配

```python
import re

print(re.findall('a.b', 'ab aab a*b a2b a牛b a\nb'))
print(re.findall('a.b', 'ab aab a*b a2b a牛b a\nb',re.DOTALL))

print(re.findall('a?b', 'ab aab abb aaaab a牛b aba**b'))

print(re.findall('a*b', 'ab aab aaab abbb'))
print(re.findall('ab*', 'ab aab aaab abbbbb'))

print(re.findall('a+b', 'ab aab aaab abbb'))

print(re.findall('a{2,4}b', 'ab aab aaab aaaaabb'))

print(re.findall('a.*b', 'ab aab a*()b')) #贪婪
print(re.findall('a.*?b', 'ab a1b a*()b, aaaaaab'))
# .*? 此时的?不是对左边的字符进行0次或者1次的匹配,
# 而只是针对.*这种贪婪匹配的模式进行一种限定:告知他要遵从非贪婪匹配 推荐使用!

# []: 括号中可以放任意一个字符,一个中括号代表一个字符
# - 在[]中表示范围,如果想要匹配上- 那么这个-符号不能放在中间.
# ^ 在[]中表示取反的意思.
print(re.findall('a.b', 'a1b a3b aeb a*b arb a_b'))
print(re.findall('a[abc]b', 'aab abb acb adb afb a_b'))
print(re.findall('a[0-9]b', 'a1b a3b aeb a*b arb a_b'))
print(re.findall('a[a-z]b', 'a1b a3b aeb a*b arb a_b'))
print(re.findall('a[a-zA-Z]b', 'aAb aWb aeb a*b arb a_b'))
print(re.findall('a[0-9][0-9]b', 'a11b a12b a34b a*b arb a_b'))
print(re.findall('a[*-+]b','a-b a*b a+b a/b a6b'))
print(re.findall('a[-*+]b','a-b a*b a+b a/b a6b'))
print(re.findall('a[^a-z]b', 'acb adb a3b a*b'))

# 分组：() 制定一个规则,将满足规则的结果匹配出来
print(re.findall('(.*?)_sb', 'cs_sb zhao_sb 日天_sb'))
print(re.findall('href="(.*?)"','<a href="http://www.baidu.com">点击</a>'))

print(re.findall('compan(y|ies)','Too many companies have gone bankrupt, and
the next one is my company'))
print(re.findall('compan(?:y|ies)','Too many companies have gone bankrupt,
and the next one is my company'))
# 分组() 中加入?: 表示将整体匹配出来而不只是()里面的内容
```



## 常用方法举例

```python
import re

# findall 全部找到返回一个列表
print(re.findall('a','aghjmnbghagjmnbafgv'))

# search 只到找到第一个匹配然后返回一个包含匹配信息的对象,该对象可以通过调用group()方法
得到匹配的字符串,如果字符串没有匹配，则返回None
print(re.search('sb|chensong', 'chensong sb sb demon 日天'))
print(re.search('chensong', 'chensong sb sb barry 日天').group())

# match：None,同search,不过在字符串开始处进行匹配,完全可以用search+^代替match
print(re.match('sb|chensong', 'chensong sb sb demon 日天'))
print(re.match('chensong', 'chensong sb sb barry 日天').group())

# split 分割 可按照任意分割符进行分割,中括号里面写的所有符号都可以进行分割
print(re.split('[:：,;；，]','1;3,c,a：3'))

# sub 替换
print(re.sub('帅哥','sb','陈松是一个帅哥'))

# complie 根据包含的正则表达式的字符串创建模式对象。可以实现更有效率的匹配。
obj = re.compile('\d{2}')
print(obj.search('abc123eeee').group())
print(obj.findall('1231232aasd'))

ret = re.finditer('\d','asd123affess32432') # finditer返回一个存放匹配结果的迭代器
print(ret)
print(next(ret).group())
print(next(ret).group())
print([i.group() for i in ret])
```

## 命名分组举例

命名分组匹配

```python
import re

ret = re.search("<(?P<tag_name>\w+)>\w+</(?P=tag_name)>","<h1>hello</h1>")
#利用?P的方式可以给匹配的东西添加一个标签名称，一次可以匹配多个内容，利用标签名称可以去取
print(ret.group('tag_name'))
print(ret.group())

ret = re.search(r"<(\w+)>\w+</\1>","<h1>hello</h1>")
# 如果不给组起名字，也可以用\序号来找到对应的组，表示要找的内容和前面的组内容一致
# 获取的匹配结果可以直接用group(序号)拿到对应的值
print(ret.group(1))
print(ret.group())
```

利用re模块实现网络爬虫

```python
# 爬取小姐姐图片
https://www.3gbizhi.com/meinv/index_1.html
```

```python

# 1.找个网站
# 2.找一下页面的规律
# 3.找一下图片链接和图片名
# 4.想办法下载
import os
import re
from urllib.request import Request, urlopen, urlretrieve

path = os.getcwd() + os.sep + "bizhi"


def download(n):
    if not os.path.isdir(path):
        os.makedirs(path)
    for i in range(1,n+1):
        url=f"https://www.3gbizhi.com/meinv/index_{str(i)}.html"
        print(url)
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.64 Safari/537.36'
        }
        request=Request(url=url,headers=headers)
        content=urlopen(request).read().decode("utf-8")
        # lazysrc="(.*?jpg)"
        # img alt="(.*?)"
        link=re.findall('lazysrc="(.*?jpg)"',content)
        name=re.findall('img alt="(.*?)"',content)
        ziplist=list(zip(name,link))
        for imgurl in ziplist:
            urlretrieve(imgurl[1],f'{path}{os.sep}{imgurl[0]}.jpg')
def main():
    n=int(input("请输入要下载的页数:"))
    download(n)
    print("下载完成，请查看文件夹")
if __name__ == '__main__':
    main()
```

```python
# 爬取豆瓣top250
https://movie.douban.com/top250?start=0&filter=
```

# shutil模块

高级的 文件、文件夹、压缩包 处理模块

## shutil.copyfileobj(fsrc, fdst[, length])

将文件内容拷贝到另一个文件中

```python
import shutil

shutil.copyfileobj(open('config','r'),open('config.new','w'))
```

## shutil.copyfile(src, dst)

拷贝文件

```python
import shutil

shutil.copyfile('config','config1') # 目标文件无需存在
```

## shutil.copymode(src, dst)

仅拷贝权限。内容、组、用户均不变

```python
import shutil

shutil.copymode('config','config1') # 目标文件必须存在
```

## shutil.copystat(src, dst)

仅拷贝状态的信息，包括：mode bits, atime, mtime, flags

```python
import shutil

shutil.copystat('config','config1') # 目标文件必须存在
```

## shutil.copy(src, dst)

拷贝文件和权限

```python
import shutil

shutil.copy('config','config1') # 目标文件必须存在
```

## shutil.ignore_patterns(*patterns)

## shutil.copytree(src, dst, symlinks=False, ignore=None)

递归的去拷贝文件夹

```python
import shutil

shutil.copytree('folder1', 'folder2', ignore=shutil.ignore_patterns('*.pyc',
'tmp*'))
# 目标目录不能存在，注意对folder2目录父级目录要有可写权限，ignore的意思是排除
# 硬链接

shutil.copytree('f1', 'f2', symlinks=True,
ignore=shutil.ignore_patterns('*.pyc', 'tmp*'))
# 软链接
```

## shutil.rmtree(path[, ignore_errors[, onerror]])

递归的去删除文件

```python
import shutil

shutil.rmtree('folder1')
```

## shutil.move(src, dst)

递归的去移动文件，它类似mv命令，其实就是重命名。

```python
import shutil

shutil.move('folder1', 'folder3')
```

## shutil.make_archive(base_name, format,...)

- 创建压缩包并返回文件路径，例如：zip、tar
  - base_name： 压缩包的文件名，也可以是压缩包的路径。只是文件名时，则保存至当前目 录，否则保存至指定路径
    - 如 data_bak =>保存至当前路径
    - 如：/tmp/data_bak =>保存至/tmp/
  - format： 压缩包种类，“zip”, “tar”, “bztar”，“gztar”
  - root_dir： 要压缩的文件夹路径（默认当前目录）
  - owner： 用户，默认当前用户
  - group： 组，默认当前组
  - logger： 用于记录日志，通常是logging.Logger对象

```python
# 将 /data 下的文件打包放置当前程序目录
import shutil
ret = shutil.make_archive("data_bak", 'gztar', root_dir='/data')

# 将 /data下的文件打包放置 /tmp/目录
import shutil
ret = shutil.make_archive("/tmp/data_bak", 'gztar', root_dir='/data')
```

shutil 对压缩包的处理是调用 ZipFile 和 TarFile 两个模块来进行的

```python
import zipfile

# 压缩
z = zipfile.ZipFile('laxi.zip', 'w')
z.write('a.log')
z.write('data.data')
z.close()

# 解压
z = zipfile.ZipFile('laxi.zip', 'r')
z.extractall(path='.')
z.close()
```

```python
import tarfile

# 压缩文件
t = tarfile.open('/tmp/egon.tar','w')
t.add('/test1/a.py',arcname='a.bak')
t.add('/test1/b.py',arcname='b.bak')
t.close()

# 解压缩文件
t = tarfile.open('/tmp/egon.tar','r')
t.extractall('/egon')
t.close()
```
