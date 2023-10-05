---
title: Python的套接字
tags: [Python学习]
categories: [Python]
index_img: /Photo/Blog_Photo/index/Python的套接字.jpg
banner_img: /Photo/Blog_Photo/Banner/Python的套接字.jpg
typora-root-url: Python的套接字
date: 2022-10-01 00:39
---

# 套接字的工作流程（基于TCP和 UDP两个协议）

## TCP和UDP对比

- TCP（Transmission Control Protocol）
  - 可靠的、面向连接的协议（eg:打电话）、传输效率低全双工通信（发送缓存&接收缓存）、 面向字节流。使用TCP的应用：Web浏览器；文件传输程序。
- UDP（User Datagram Protocol）
  - 不可靠的、无连接的服务，传输效率高（发送前时延小），一对一、一对多、多对一、多对 多、面向报文(数据包)，尽最大努力服务，无拥塞控制。使用UDP的应用：域名系统 (DNS)； 视频流；IP语音(VoIP)。

## TCP协议下的socket

![TCP过程示意图](image-20220712140039302.png)

服务器端先初始化Socket，然后与端口绑定(bind)，对端口进行监听(listen)，调用accept阻塞，等待客 户端连接。在这时如果有个客户端初始化一个Socket，然后连接服务器(connect)，如果连接成功，这时 客户端与服务器端的连接就建立了。客户端发送数据请求，服务器端接收请求并处理请求，然后把回应 数据发送给客户端，客户端读取数据，最后关闭连接，一次交互结束

```python
import socket
# 初始化格式如下
socket.socket(socket_family,socket_type,protocal=0)
# socket_family 可以是 AF_UNIX 或 AF_INET。socket_type 可以是 SOCK_STREAM 或
SOCK_DGRAM。protocol 一般不填,默认值为 0。
# 获取tcp/ip套接字
tcpSock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 获取udp/ip套接字
udpSock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# 由于 socket 模块中有太多的属性。我们在这里破例使用了'from module import *'语句。使
用 'from socket import *',我们就把 socket 模块里的所有属性都带到我们的命名空间里了,这
样能 大幅减短我们的代码。
# 例如
tcpSock = socket(AF_INET, SOCK_STREAM)
```

**服务端**套接字函数

| s.bind()   | 绑定(主机,端口号)到套接字                    |
| ---------- | -------------------------------------------- |
| s.listen() | 开始TCP监听                                  |
| s.accept() | 被动接受TCP客户的连接,(阻塞式)等待连接的到来 |

**客户端**套接字函数

| s.connect()    | s.connect_ex()                                          |
| -------------- | ------------------------------------------------------- |
| s.connect_ex() | connect()函数的扩展版本,出错时返回出错码,而不是抛出异常 |

公共用途的套接字函数

| s.recv()        | 接收TCP数据                                                  |
| --------------- | ------------------------------------------------------------ |
| s.send()        | 发送TCP数据(send在待发送数据量大于己端缓存区剩余空间时,数据丢失,不 会发完) |
| s.sendall()     | 发送完整的TCP数据(本质就是循环调用send,sendall在待发送数据量大于己 端缓存区剩余空间时,数据不丢失,循环调用send直到发完) |
| s.recvfrom()    | 接收UDP数据                                                  |
| s.sendto()      | 发送UDP数据                                                  |
| s.getpeername() | 连接到当前套接字的远端的地址                                 |
| s.getsockname() | 当前套接字的地址                                             |
| s.getsockopt()  | 返回指定套接字的参数                                         |
| s.setsockopt()  | 设置指定套接字的参数                                         |
| s.close()       | 关闭套接字                                                   |

面向锁的套接字方法

| s.setblocking() | 设置套接字的阻塞与非阻塞模式 |
| --------------- | ---------------------------- |
| s.settimeout()  | 设置阻塞套接字操作的超时时间 |
| s.gettimeout()  | 得到阻塞套接字操作的超时时间 |

面向文件的套接字的函数

| s.fileno()   | 套接字的文件描述符           |
| ------------ | ---------------------------- |
| s.makefile() | 创建一个与该套接字相关的文件 |

第一版，单个客户端与服务端通信

服务端

```python
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM) # 买电话

phone.bind(('127.0.0.1',8080)) # 0 ~ 65535 1024之前系统分配好的端口 绑定电话卡

phone.listen(5) # 同一时刻有5个请求，但是可以有N多个链接。 开机。


conn, client_addr = phone.accept() # 接电话
print(conn, client_addr, sep='\n')

from_client_data = conn.recv(1024) # 一次接收的最大限制 bytes
print(from_client_data.decode('utf-8'))

conn.send(from_client_data.upper())

conn.close() # 挂电话

phone.close() # 关机
```

客户端

```python
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM) # 买电话

phone.connect(('127.0.0.1',8080)) # 与客户端建立连接， 拨号

phone.send('hello'.encode('utf-8'))

from_server_data = phone.recv(1024)

print(from_server_data)

phone.close() # 挂电话
```

第二版，通信循环

服务端

```python
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
phone.bind(('127.0.0.1',8080))
phone.listen(5)

conn,client_addr = phone.accept()
print(conn,client_addr,sep='\n')

while 1:
	try:
		from_client_data = conn.recv(1024)
		print(from_client_data.decode('utf-8'))

		conn.send(from_client_data.upper())
	except ConnectionResetError:
		break

conn.close()
phone.close()
```

客户端

```python
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
phone.connect(('127.0.0.1',8080))

while 1:
    client_data = input('>>> ')
    phone.send(client_data.encode('utf-8'))
    
    from_server_data = phone.recv(1024)
    print(from_server_data.decode('utf-8'))
    
phone.close()
```

第三版， 通信，连接循环

服务端

```python
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
phone.bind(('127.0.0.1',8080))

phone.listen(5)

while 1:
    conn,client_addr = phone.accept()
    print(conn,client_addr,sep='\n')
    
	while 1:
		try:
			from_client_data = conn.recv(1024)
			if not from_client_data:
				break
			print(from_client_data.decode('utf-8'))
			
			conn.send(from_client_data.upper())
		except:
			break

conn.close()
phone.close()
```

客户端

```python
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
phone.connect(('127.0.0.1',8080))

while 1:
    client_data = input('>>> ')
    phone.send(client_data.encode('utf-8'))
    if client_data == 'q':break
    
    from_server_data = phone.recv(1024)
    print(from_server_data.decode('utf-8'))
    
phone.close()
```

远程执行命令的示例：

```python
import socket
import subprocess

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

phone.bind(('127.0.0.1', 8080))

phone.listen(5)

while 1: # 循环连接客户端
    conn, client_addr = phone.accept()
    print(client_addr)
	while 1:
		try:
            cmd = conn.recv(1024)
            ret = subprocess.Popen(cmd.decode('utf-8'), shell=True,
stdout=subprocess.PIPE, stderr=subprocess.PIPE)
            correct_msg = ret.stdout.read()
            error_msg = ret.stderr.read()
            conn.send(correct_msg + error_msg)
		except ConnectionResetError:
			break
			
conn.close()
phone.close()
```

客户端

```python
import socket

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM) # 买电话

phone.connect(('127.0.0.1', 8080)) # 与客户端建立连接， 拨号

while 1:
	cmd = input('>>>')
	phone.send(cmd.encode('utf-8'))
	
	from_server_data = phone.recv(1024)
	
	print(from_server_data.decode('gbk'))
	
phone.close() # 挂电话
```

# UDP协议下的socket

udp是无链接的，先启动哪一端都不会报错

![UDP的过程示意图](image-20220712141427062.png)

服务器端先初始化Socket，然后与端口绑定(bind)，recvform接收消息，这个消息有两项，消息内容和 对方客户端的地址，然后回复消息时也要带着你收到的这个客户端的地址，发送回去，最后关闭连接， 一次交互结束

服务端

```python
import socket
udp_sk = socket.socket(type=socket.SOCK_DGRAM) #创建一个服务器的套接字
udp_sk.bind(('127.0.0.1',9000)) #绑定服务器套接字
msg,addr = udp_sk.recvfrom(1024)
print(msg)
udp_sk.sendto(b'hi',addr) # 对话(接收与发送)
udp_sk.close() # 关闭服务器套接字
```

客户端

```python
import socket
ip_port=('127.0.0.1',9000)
udp_sk=socket.socket(type=socket.SOCK_DGRAM)
udp_sk.sendto(b'hello',ip_port)
back_msg,addr=udp_sk.recvfrom(1024)
print(back_msg.decode('utf-8'),addr)
```

类似于qq聊天的代码示例

服务端

```python
import socket
ip_port=('127.0.0.1',8081)
udp_server_sock=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
#DGRAM:datagram 数据报文的意思，象征着UDP协议的通信方式
udp_server_sock.bind(ip_port)#你对外提供服务的端口就是这一个，所有的客户端都是通过这个端口和你进行通信的

while True:
    qq_msg,addr=udp_server_sock.recvfrom(1024)# 阻塞状态，等待接收消息
    print('来自[%s:%s]的一条消息:\033[1;34;43m%s\033[0m' %(addr[0],addr[1],qq_msg.decode('utf-8')))
	back_msg=input('回复消息: ').strip()

	udp_server_sock.sendto(back_msg.encode('utf-8'),addr)
```

客户端

```python
import socket
BUFSIZE=1024
udp_client_socket=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)

qq_name_dic={
    'taibai':('127.0.0.1',8081),
    'Jedan':('127.0.0.1',8081),
    'Jack':('127.0.0.1',8081),
    'John':('127.0.0.1',8081),
}
while True:
    # 选择聊天对象
    while 1:
        qq_name=input('请选择聊天对象: ').strip()
        if qq_name not in qq_name_dic:
            print('没有这个聊天对象')
            continue
        break
        
	# 聊天过程
	while True:
		msg='发给'+ qq_name + ': ' + input('请输入消息,回车发送,输入q结束和他的聊天: ').strip()
		if msg == 'q':break
		if not msg:continue
		udp_client_socket.sendto(msg.encode('utf-8'),qq_name_dic[qq_name])#必须带着自己的地址，这就是UDP不一样的地方，不需要建立连接，但是要带着自己的地址给服务端，否则服务端无法判断是谁给我发的消息，并且不知道该把消息回复到什么地方，因为我们之间没有建立连接通道

        back_msg,addr=udp_client_socket.recvfrom(BUFSIZE)# 同样也是阻塞状态，等待接收消息
		print('来自[%s:%s]的一条消息:\033[1;34;43m%s\033[0m' %(addr[0],addr[1],back_msg.decode('utf-8')))
        
udp_client_socket.close()
```

自制时间服务器

服务端

```python
from socket import *
from time import strftime
import time
ip_port = ('127.0.0.1', 9000)
bufsize = 1024

tcp_server = socket(AF_INET, SOCK_DGRAM)
tcp_server.setsockopt(SOL_SOCKET,SO_REUSEADDR,1)
tcp_server.bind(ip_port)

while True:
    msg, addr = tcp_server.recvfrom(bufsize)
    print('===>', msg.decode('utf-8'))
    stru_time = time.localtime() #当前的结构化时间
    if not msg:
    	time_fmt = '%Y-%m-%d %X'
    else:
    	time_fmt = msg.decode('utf-8')
    back_msg = strftime(time_fmt,stru_time)
    print(back_msg,type(back_msg))
    tcp_server.sendto(back_msg.encode('utf-8'), addr)
tcp_server.close()
```

客户端

```python
from socket import *
ip_port=('127.0.0.1',9000)
bufsize=1024

tcp_client=socket(AF_INET,SOCK_DGRAM)

while True:
    msg=input('请输入时间格式(例%Y %m %d)>>: ').strip()
    tcp_client.sendto(msg.encode('utf-8'),ip_port)
    
    data=tcp_client.recv(bufsize)
    print('当前日期：',str(data,encoding='utf-8'))
```

# 粘包

![粘包过程图](image-20220712142023973.png)

每个 socket 被创建后，都会分配两个缓冲区，输入缓冲区和输出缓冲区。

write()/send() 并不立即向网络中传输数据，而是先将数据写入缓冲区中，再由TCP协议将数据从缓冲区 发送到目标机器。一旦将数据写入到缓冲区，函数就可以成功返回，不管它们有没有到达目标机器，也 不管它们何时被发送到网络，这些都是TCP协议负责的事情。

TCP协议独立于 write()/send() 函数，数据有可能刚被写入缓冲区就发送到网络，也可能在缓冲区中不断 积压，多次写入的数据被一次性发送到网络，这取决于当时的网络情况、当前线程是否空闲等诸多因 素，不由程序员控制。

read()/recv() 函数也是如此，也从输入缓冲区中读取数据，而不是直接从网络中读取。

这些I/O缓冲区特性可整理如下：

1. I/O缓冲区在每个TCP套接字中单独存在；
2. I/O缓冲区在创建套接字时自动生成；
3. 即使关闭套接字也会继续传送输出缓冲区中遗留的数据；
4. 关闭套接字将丢失输入缓冲区中的数据。

## 两种情况下会发生粘包

1. 接收方没有及时接收缓冲区的包，造成多个包接收（客户端发送了一段数据，服务端只收了一小部 分，服务端下次再收的时候还是从缓冲区拿上次遗留的数据，产生粘包）

服务端

```python
import socket
import subprocess

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

phone.bind(('127.0.0.1', 8080))

phone.listen(5)

while 1: # 循环连接客户端
    conn, client_addr = phone.accept()
    print(client_addr)
    
	while 1:
		try:
			cmd = conn.recv(1024)
			ret = subprocess.Popen(cmd.decode('utf-8'), shell=True,stdout=subprocess.PIPE, stderr=subprocess.PIPE)
            correct_msg = ret.stdout.read()
            error_msg = ret.stderr.read()
            conn.send(correct_msg + error_msg)
		except ConnectionResetError:
			break
			
conn.close()
phone.close()
```



发送端需要等缓冲区满才发送出去，造成粘包（发送数据时间间隔很短，数据也很小，会合到一 起，产生粘包）

服务端

```py
import socket


phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

phone.bind(('127.0.0.1', 8080))

phone.listen(5)

conn, client_addr = phone.accept()

frist_data = conn.recv(1024)
print('1:',frist_data.decode('utf-8')) # 1: helloworld
second_data = conn.recv(1024)
print('2:',second_data.decode('utf-8'))


conn.close()
phone.close()
```



客户端

```python
import socket

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

phone.connect(('127.0.0.1', 8080))

phone.send(b'hello')
phone.send(b'world')

phone.close()
```

# 粘包的解决方案

## struct模块

该模块可以把一个类型，如数字，转成固定长度的bytes

![Struct转换对应图](image-20220712142819170.png)

```py
import struct
# 将一个数字转化成等长度的bytes类型。
ret = struct.pack('i', 183346)
print(ret, type(ret), len(ret))

# 通过unpack反解回来
ret1 = struct.unpack('i',ret)[0]
print(ret1, type(ret1), len(ret1))

# 但是通过struct 处理不能处理太大
```



方案一:

```python
import socket
import subprocess
import struct

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

phone.bind(('127.0.0.1', 8080))

phone.listen(5)

while 1:
    conn, client_addr = phone.accept()
    print(client_addr)
    
while 1:
	try:
        cmd = conn.recv(1024)
        ret = subprocess.Popen(cmd.decode('utf-8'), shell=True,stdout=subprocess.PIPE, stderr=subprocess.PIPE)
        correct_msg = ret.stdout.read()
        error_msg = ret.stderr.read()

		# 1 制作固定报头
        total_size = len(correct_msg) + len(error_msg)
        header = struct.pack('i', total_size)

		# 2 发送报头
		conn.send(header)

        # 发送真实数据：
        conn.send(correct_msg)
        conn.send(error_msg)
     except ConnectionResetError:
        break
        
conn.close()
phone.close()

# 但是low版本有问题：
# 1，报头不只有总数据大小，而是还应该有MD5数据，文件名等等一些数据。
# 2，通过struct模块直接数据处理，不能处理太大。
```

客户端

```py
import socket
import struct

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

phone.connect(('127.0.0.1', 8080))

while 1:
    cmd = input('>>>').strip()
    if not cmd: continue
    phone.send(cmd.encode('utf-8'))
    
	# 1，接收固定报头
	header = phone.recv(4)

	# 2，解析报头
	total_size = struct.unpack('i', header)[0]
	
	# 3，根据报头信息，接收真实数据
	recv_size = 0
	res = b''

	while recv_size < total_size:
		recv_data = phone.recv(1024)
		res += recv_data
		recv_size += len(recv_data)

	print(res.decode('gbk'))

phone.close()
```



方案二：可自定制报头

```
整个流程的大致解释：
我们可以把报头做成字典，字典里包含将要发送的真实数据的描述信息(大小啊之类的)，然后json序列
化，然后用struck将序列化后的数据长度打包成4个字节。
我们在网络上传输的所有数据 都叫做数据包，数据包里的所有数据都叫做报文，报文里面不止有你的数
据，还有ip地址、mac地址、端口号等等，其实所有的报文都有报头，这个报头是协议规定的，看一下

发送时：
先发报头长度
再编码报头内容然后发送
最后发真实内容

接收时：
先手报头长度，用struct取出来
根据取出的长度收取报头内容，然后解码，反序列化
从反序列化的结果中取出待取数据的描述信息，然后去取真实的数据内容
```

服务端

```py
import socket
import subprocess
import struct
import json

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

phone.bind(('127.0.0.1', 8080))

phone.listen(5)

while 1:
    conn, client_addr = phone.accept()
    print(client_addr)

	while 1:
		try:
			cmd = conn.recv(1024)
			ret = subprocess.Popen(cmd.decode('utf-8'), shell=True,stdout=subprocess.PIPE, stderr=subprocess.PIPE)
			correct_msg = ret.stdout.read()
			error_msg = ret.stderr.read()

             # 1 制作固定报头
             total_size = len(correct_msg) + len(error_msg)

			header_dict = {
                'md5': 'fdsaf2143254f',
                'file_name': 'f1.txt',
                'total_size': total_size,
			}
			
             header_dict_json = json.dumps(header_dict) # str
             bytes_headers = header_dict_json.encode('utf-8')

			header_size = len(bytes_headers)

			header = struct.pack('i', header_size)

            # 2 发送报头长度
            conn.send(header)

            # 3 发送报头
            conn.send(bytes_headers)

            # 4 发送真实数据：
            conn.send(correct_msg)
            conn.send(error_msg)
		except ConnectionResetError:
			break

conn.close()
phone.close()
```

客户端

```py
import socket
import struct
import json

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

phone.connect(('127.0.0.1', 8080))

while 1:
    cmd = input('>>>').strip()
    if not cmd: continue
    phone.send(cmd.encode('utf-8'))
    
    # 1，接收固定报头
    header_size = struct.unpack('i', phone.recv(4))[0]

    # 2，解析报头长度
    header_bytes = phone.recv(header_size)
    
	header_dict = json.loads(header_bytes.decode('utf-8'))
	
    # 3,收取报头
    total_size = header_dict['total_size']
    
    # 3，根据报头信息，接收真实数据
    recv_size = 0
    res = b''

	while recv_size < total_size:
		recv_data = phone.recv(1024)
		res += recv_data
		recv_size += len(recv_data)
	
	print(res.decode('gbk'))

phone.close()
```

FTP上传下载文件的代码（简单版）

服务端

```python
import socket
import struct
import json
sk = socket.socket()
# buffer = 4096 # 当双方的这个接收发送的大小比较大的时候，就像这个4096，就会丢数据，这
个等我查一下再告诉大家，改小了就ok的，在linux上也是ok的。
buffer = 1024 #每次接收数据的大小
sk.bind(('127.0.0.1',8090))
sk.listen()

conn,addr = sk.accept()
#接收
head_len = conn.recv(4)
head_len = struct.unpack('i',head_len)[0] #解包

json_head = conn.recv(head_len).decode('utf-8') #反序列化
head = json.loads(json_head)
filesize = head['filesize']

with open(head['filename'],'wb') as f:
	while filesize:
		if filesize >= buffer: #>=是因为如果刚好等于的情况出现也是可以的。
             content = conn.recv(buffer)
			f.write(content)
			filesize -= buffer
		else:
			content = conn.recv(buffer)
			f.write(content)
			break

conn.close()
sk.close()
```

客户端

```py
import os
import json
import socket
import struct
sk = socket.socket()
sk.connect(('127.0.0.1',8090))
buffer = 1024 #读取文件的时候，每次读取的大小
head = {
            'filepath':r'C:\Users\Aaron\Desktop\新建文件夹', #需要下载的文件路径，也就是文件所在的文件夹
            'filename':'config', #改成上面filepath下的一个文件
            'filesize':None,
       }
       
file_path = os.path.join(head['filepath'],head['filename'])
filesize = os.path.getsize(file_path)
head['filesize'] = filesize

# json_head = json.dumps(head,ensure_ascii=False) #字典转换成字符串
json_head = json.dumps(head) #字典转换成字符串
bytes_head = json_head.encode('utf-8') #字符串转换成bytes类型
print(json_head)
print(bytes_head)

#计算head的长度，因为接收端先接收我们自己定制的报头，对吧
head_len = len(bytes_head) #报头长度
pack_len = struct.pack('i',head_len)
print(head_len)
print(pack_len)
sk.send(pack_len) #先发送报头长度
sk.send(bytes_head) #再发送bytes类型的报头

#即便是视频文件，也是可以按行来读取的，也可以readline，也可以for循环，但是读取出来的数据
大小就不固定了，影响效率，有可能读的比较小，也可能很大，像视频文件一般都是一行的二进制字节
流。
#所有我们可以用read，设定一个一次读取内容的大小，一边读一边发，一边收一边写
with open(file_path,'rb') as f:
	while filesize:
		if filesize >= buffer: #>=是因为如果刚好等于的情况出现也是可以的。
            content = f.read(buffer) #每次读取出来的内容
            sk.send(content)
            filesize -= buffer #每次减去读取的大小
        else: #那么说明剩余的不够一次读取的大小了，那么只要把剩下的读取出来发送过去就行了
			content = f.read(filesize)
			sk.send(content)
			break
sk.close()
```
