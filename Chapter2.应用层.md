## 2.应用层

### 2.1应用层协议原理

#### 2.1.1网络应用程序体系

两种：

客户-服务器体系结构：Web、FTP、Telnet和电子邮件

端对端（P2P）体系结构：对服务器没有依赖或比较小依赖，应用程序在间断连接的主机对直接使用直接通信，这些主机称为对等方。例如，文件共享、对等方协作下载加速器（迅雷）、因特网电话等。

#### 2.1.2进程通信

在不同端系统上的进程，跨计算机网络交换报文来相互通信。

**1.进程与计算机网络之间的接口**

进程通过套接字软件接口向网络发送报文和从网络接收报文。

**2.进程寻址**

标识接收进程的信息：主机的地址 （主机由IP标识）、 定义在目的主机上的接收进程（由端口号标识）的标识符

![](F:\Desktop\Typora\Computer-Network\应用程序、套接字和下面的协议.png)

#### 2.1.3可供应用程序使用的运输服务

可靠数据传输、吞吐量、定时和安全性

#### 2.1.4因特网提供的运输服务

两个运输层协议UDP和TCP

**TCP服务**：

1.面向连接，握手过程，之后一个TCP连接就在两个进程的套接字之间建立了。连接是全双工的（连接双方的进程可以在此连接上同时进行报文收发）

2.可靠的数据传送，无差错、按适当顺序交付所有发送的数据。

3.阻塞控制机制和分组开销

#### 2.1.5应用层协议

应用层协议定义了运行在不同端系统上的应用程序进程如何相互交换报文：

 交换报文的类型，如请求/响应报文

 各种报文类型的语法，如报文中的各个字段及这些字段如何描述

 字段的语义，即这些字段中包含的信息的含义

 一个进程何时以及如何发送报文，对报文进行响应的规则。

有些应用层协议由RFC文档定义，位于公共域中（如Web的应用层协议HTTP），有些是专用的（如Skype使用专用的应用层协议）

### 2.2Web和HTTP

#### 2.2.1 HTTP

http定义了报文的结构以及客户和服务器进行报文交换的方式。

HTTP是无状态协议，因为HTTP服务器并不保存关于客户的任何信息。

#### 2.2.2非持续连接和持续连接

客户-服务器的交互是经TCP进行的，每个请求/响应对是经一个单独TCP连接发送，这是非持续连接。

所有的请求及响应经相同的TCP连接发送，则是持续连接。

#### 2.2.3 HTTP报文格式

**请求报文**

![](F:\Desktop\Typora\Computer-Network\HTTP报文格式.png)

请求行 ：请求方法 GEP/POST  URL  HTTP版本

首部行

![](F:\Desktop\Typora\Computer-Network\HTTP报文格式1.png)



**响应报文**

状态行 ： 版本、状态码、状态码对应的描述

首部行

空行

实体行

![](F:\Desktop\Typora\Computer-Network\HTTP响应报文.png)



​         常见状态码和描述： 

200 OK: 表示请求成功。

301 Moved Permanently: 请求的对象已经被永久转移了，新的URL定义在响应报文的Location：首部行中。

400 Bad Request：该请求不能被服务器理解

404 Not Found：被请求的文档不在服务器上

505 HTTP Version Not Supported：服务器不支持请求报文使用的HTTP协议版本

#### 2.2.4  用户与服务器的交互：Cookie

因为HTTP服务器是无状态的，cookie用于Web站点对用户进行跟踪。

#### 2.2.5 Web 缓存

Web缓存器（代理服务器）

假设游览器正在请求对象http://www.someschool.edu/campus.gif,将会发送：

1.浏览器建立一个到Web缓存器的TCP连接，并向Web缓存器中的对象发送一个HTTP请求

2.Web缓存器进行检查，看本地是都存储了该对象副本。若有，Web缓存器就向客户浏览器用HTTP响应报文返回该对象

3.若没有，就打开一个与该对象的初始服务器（如www.someschool.edu）的TCP连接。Web缓存器在这个缓存器到服务器的TCP连接上发送一个对该对象的HTTP请求。收到请求后，初始服务器发送HTTP响应

4.Web缓存器收到该对象，在本地存储空间存储一份副本，并向客户的浏览器用HTTP响应报文发送该副本。

![](F:\Desktop\Typora\Computer-Network\缓存器.png)



#### 2.2.6 条件GET方法

高速缓存能减少用户感受到的响应时间，但存在缓存器中的对象副本可能是旧的。

HTTP协议中一种**条件GET**方法 机制允许缓存器证实它的对象是最新的。



### 2.3文件传输协议：FTP

FTP使用了两个并行的TCP连接来传输文件，一个是**控制连接**（传输控制信息，如用户标识、口令等），一个是 **数据连接** （实际发送一个文件）。

FTP服务器必须在整个绘画期间保留用户的状态。

### 2.4因特网中的电子邮件

电子邮件系统组成部分：用户代理、邮件服务器、简单邮件传输协议（SMTP）

![](F:\Desktop\Typora\Computer-Network\电子邮件系统.png)



#### 2.4.1 SMTP

用于从发送方的邮件服务器发送报文到接收方的邮件服务器。

与HTTP的区别：HTTP主要是拉协议，用户使用HTTP拉取在Web服务器上装载的信息。 SMTP是推协议，即发送邮件服务器把文件推向接收邮件服务器。

SMTP要求每个报文（包括它们的体）使用7比特ASCII码格式。

SMTP把所有报文对象放在一个报文中。

### 2.5DNS:因特网的目录服务

#### 2.5.1DNS提供的服务

识别主机方式：主机名/ IP地址

能进行主机名到 IP地址转换的目录服务，**就是域名系统（DNS）**。  DNS协议运行在UDP之上，53号端口。

#### 2.5.2DNS工作机理

分布式的设计，三种类型：根DNS服务器、顶级域DNS服务器（vom、org、net、edu、uk、fr）和权威DNS服务器。

**递归查询** 和 **迭代查询**

实践中，从请求主机到本地DNS服务器的查询是递归，其余查询是迭代的。

**DNS缓存**

#### 2.5.3DNS记录和报文

共同实现DNS分布式数据库的所有DNS服务器**存储了资源记录（RR）**，RR提供了主机名到IP地址的映射。

### 2.6 P2P应用

Web、电子邮件和DNS都采用C/S体系结构，极大地依赖于总是打开的基础设施服务器。

P2P体系结构，对服务器有最小的（或没有）依赖，成对间歇连接的主机（对等方）彼此直接通信。

#### 2.6.1 P2P文件分发

P2P的内在自扩展性。（对等方间相互分发文件，不只是由单一服务器进行分发）

P2P文件共享协议BitTorrent。

任何对等方讲允许在数据库中插入新键值对，这样一种分布式数据库被称为分布式散列表（DHT）。

### 2.7 UDP编程

UDPClient.py

```python
from socket import *
import sys
serverName="127.0.0.1"
serverPort=12003  #端口号写为str时报错：TypeError: an integer is required (got type str)
clientSocket=socket(AF_INET,SOCK_DGRAM)  #第一个参数指示了地址簇，AF_INET指示底层网络用IP4，第二个参数指示该套接字类型是UDP套接字
message=input('Input lowercase sentence:')
clientSocket.sendto(message.encode('utf-8'),(serverName,serverPort))  # 不设置encode是报错：TypeError: a bytes-like object is required, not 'str'
modifiedMessage,serverAddress=clientSocket.recvfrom(2048)
print(modifiedMessage)
clientSocket.close()
```

UDPServer.py

```python
from socket import *
serverPort=12003
serverSocket=socket(AF_INET,SOCK_DGRAM)
serverSocket.bind(('',serverPort))  #绑定端口
print("The server is ready to receive")
while 1 :
    message,clientAdress=serverSocket.recvfrom(2048)
    modifiedMessage=message.upper()
    serverSocket.sendto(modifiedMessage,clientAdress)
```

`s.sendto(string[,flag],address)发送UDP数据。将数据发送到套接字，address是形式为（ipaddr，port）的元组，指定远程地址。返回值是发送的字节数。destPort的类型应该为int类型.

### 2.8 TCP编程

TCPClient.py

```python
from socket import *
serverName='127.0.0.1'
serverPort=11998
clientSocket=socket(AF_INET,SOCK_STREAM)
clientSocket.connect((serverName,serverPort))
sentence=input('Input lowercase sentence:')
clientSocket.send(sentence.encode('utf-8'))
modifiedSentence=clientSocket.recv(1024)
print('From server:',modifiedSentence)
clientSocket.close()
```

TCPServer.py

```python
from socket import *
serverPort=11998
serverSocket=socket(AF_INET,SOCK_STREAM)
serverSocket.bind(('',serverPort))
serverSocket.listen(1)
print("The server is ready to receive")
while 1 :
    connectionSocket,addr=serverSocket.accept()
    sentence=connectionSocket.recv(1024)
    capitalizedSentence=sentence.upper()
    connectionSocket.send(capitalizedSentence)
    connectionSocket.close()

```

































































































































































