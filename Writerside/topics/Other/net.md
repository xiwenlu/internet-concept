# http网络

1. 端口号是用于标识进程的逻辑地址，不同进程的标识。有效端口：0 ~ 65535，其中0 ~ 1023 系统使用或保留端口。
2. InetAddress是Java对IP地址（包括IPv4和IPv6）的高层表示。在Java中，InetAddress类用于封装IP地址，以便于对IP地址的获取和操作。
3. IP地址（Internet Protocol Address）：IP地址是IP协议提供的一种统一的地址格式，它为互联网上的每一个网络和每一台主机分配一个逻辑地址，以此来屏蔽物理地址的差异。
4. 本地回环地址：127.0.0.1，主机名：localhost。


## get和post的区别
1. get是从服务器上获取数据，post是向服务器传送数据，
2. get传送的数据量较小，不能大于2KB。post传送的数据量较大，一般被默认为不受限制。
3. get安全性非常低，post安全性较高。但是执行效率却比Post方法好。
4. 在进行文件上传时只能使用post而不能是get。
5. get传输的数据在地址栏可见，post的数据不可见。
6. 对于get方式，服务器端用Request.QueryString获取变量的值；对于post方式，服务器端用Request.Form获取提交的数据。


## Http协议原理 {id="http_1"}
HTTP是一个超文本传输协议，属于OSI七层模型的应用层，由请求和响应构成，是一个标准的客户端服务器模型。HTTP是无状态的也就是说同一个客户端的这次请求和上次请求是没有对应关系。

## http的工作流程
当发送一个http请求时，首先客户机和服务器会建立连接，之后发送请求到服务器，请求中包含了要访问的url地址，请求的方式（get/post），以及要传递的参数和头信息，服务器接到请求后会进行响应，包括状态行，状态码，响应头，以及要响应的主体内容。客户端接收到请求后将其展示到浏览器上然后断开和服务器端的连接。


## 网络编程
网络模型
    OSI参考模型
    TCP/IP参考模型
网络通讯要素
    IP地址
    端口号
    传输协议

## 网络参考模型
<img src="net_model.png" alt=""/>

## 常用的服务器端口号

|   协议   | 端口号 |
|:------:|:---:|
|  HTTP  | 80  |
|  SMTP  | 25  |
|  POP3  | 110 |
|  FTP   | 21  |
| Telnet | 23  |


## TCP和UDP的区别

### UDP
1. 将数据及源和目的封装成数据包中，不需要建立连接
2. 每个数据报的大小在限制在64k内
3. 因无连接，是不可靠协议
4. 不需要建立连接，速度快

### TCP
1. 建立连接，形成传输数据的通道。
2. 在连接中进行大数据量传输
3. 通过三次握手完成连接，是可靠协议
4. 必须建立连接，效率会稍低

## 什么是Socket？
1. Socket就是为网络服务提供的一种机制。
2. 通信的两端都有Socket。
3. 网络通信其实就是Socket间的通信。
4. 数据在两个Socket间通过IO传输。

## UDP如何传输？ {id="udp_1"}
1. DatagramSocket与DatagramPacket
2. 建立发送端，接收端。
3. 建立数据包。
4. 调用Socket的发送接收方法。
5. 关闭Socket。
6. 发送端与接收端是两个独立的运行程序。

在发送端，要在数据包对象中明确目的地IP及端口。在接收端，要指定监听的端口。


## 什么是UDP聊天程序？ {id="udp_2"}
通过键盘录入获取要发送的信息。
将发送和接收分别封装到两个线程中。

## TCP如何传输？ {id="tcp_1"}
1. Socket和ServerSocket
2. 建立客户端和服务器端
3. 建立连接后，通过Socket中的IO流进行数据的传输
4. 关闭socket
5. 上传： 源：字节流1.jpg  目的：网络流

同样，客户端与服务器端是两个独立的应用程序。


## Tcp传输最容易出现的问题 {id="tcp_2"}
客户端连接上服务端，两端都在等待，没有任何数据传输。

通过例程分析： 因为read方法或者readLine方法是阻塞式。

解决办法： 自定义结束标记；使用shutdownInput，shutdownOutput方法。

## 客户端基本思路
客户端需要明确服务器的ip地址以及端口，这样才可以去试着建立连接，如果连接失败，会出现异常。

连接成功，说明客户端与服务端建立了通道，那么通过IO流就可以进行数据的传输，而Socket对象已经提供了输入流和输出流对象，通过getInputStream(),getOutputStream()获取即可。

与服务端通讯结束后，关闭Socket。


## 服务端基本思路
1. 服务端需要明确它要处理的数据是从哪个端口进入的。
2. 当有客户端访问时，要明确是哪个客户端，可通过accept()获取已连接的客户端对象，并通过该对象与客户端通过IO流进行数据传输。
3. 当该客户端访问结束，关闭该客户端。