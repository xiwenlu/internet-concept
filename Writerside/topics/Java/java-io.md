# io流

## 什么是java的io流？ {id="java-io_what"}
Java的IO流是一种处理数据的机制，它允许程序以流的形式进行数据的读取和写入操作。IO流可以处理各种类型的数据，包括文本、图片、音频、视频等。它可以帮助程序员方便地读取和写入数据，同时还可以控制数据的读写方式和处理方式。

## java的io流有哪些类型？ {id="java-io_type"}

### 根据数据流的流向
IO流分为输入流和输出流。输入流用于从数据源读取数据，输出流用于将数据写入到目标数据源。

### 根据处理数据单位的不同
1. 字节流主要处理二进制数据，如InputStream和OutputStream
2. 字符流主要处理文本数据，如Reader和Writer。这些流都是抽象类，具体使用时需要使用它们的子类。

### 根据实现功能的不同
IO流分为节点流和处理流。节点流直接与数据源相连，读写操作方便；处理流与节点流一起使用，在节点流的基础上再套接一层，使得读写操作更加高效。

## java中如何操作io流？
1. IO流可以使用不同的方式打开文件，如使用File对象或文件路径名。使用IO流可以方便地读取或写入文件，同时还可以控制数据的读写方式，如缓冲、转换编码等。
2. 除了基本的IO流外，Java还提供了其他一些高级IO类和工具类，如RandomAccessFile、DataInputStream和DataOutputStream等。这些类和工具类可以帮助程序员更方便地处理各种类型的文件和数据。

## 文件路径为什么使用双斜杠？
在Java中，我们经常使用双斜杠（\）来表示文件路径，这是因为反斜杠（\）在Java字符串中是一个转义字符。这意味着它用于引入特殊的字符序列。例如，\n表示新行，\t表示制表符等。

## 什么是字符流的缓冲区？
1. 字符流的缓冲区是一种将字符数据存储在内存中的区域，通常用于提高读写操作的效率。它通常封装了数组，允许将数据存入，再一次性取出。
2. 这种技术可以避免频繁地从文件或其他数据源中读取或写入数据，从而减少系统资源的消耗，提高程序的性能。
3. 在Java中，BufferedReader和BufferedWriter等类提供了字符流的缓冲区功能。

## ftp服务器和文件服务器的区别？
FTP服务器和文件服务器都可以用于存储、管理和共享文件。
1. 协议：文件服务器通常使用SMB/CIFS或NFS等协议，而FTP服务器则使用FTP协议。这意味着，使用文件服务器时，用户可以通过网络共享文件和文件夹，并像在本地计算机上一样访问和管理文件。而使用FTP服务器时，用户需要使用FTP客户端软件连接到服务器，才能够上传、下载、删除、重命名等操作文件。
2. 文件传输方式：当使用FTP服务器时，必须使用FTP客户端来完成文件传输，这需要手动输入FTP服务器的地址和连接信息。登录账户后，可以将文件直接从FTP服务器中上传或下载到计算机中。然而，对于文件服务器，除了可以使用FTP协议进行上传和下载外，还可以直接通过文件服务器的共享文件夹来完成这些任务。只要在同一网络中且共享文件夹的权限允许，就可以简单地将文件从文件服务器上传和下载到计算机中。
3. 可访问性：文件服务器是本地服务器，因此只能在企业内部网络中访问。如果基本数据只能在办公室范围内访问，这将严重限制公司进行远程工作的能力。相反，如果公司使用FTP服务器，则只要能上网并登录后，就可以从世界任何地方访问文件。
4. 安全要求：由于FTP服务器上的文件是共享的，并因此暴露在网络（外网）中，所以需要多层安全保护和加密来确保数据安全。而文件服务器仍然需要安全措施，但如何保持数据安全有不同的考虑因素。
5. 功能和应用场景：文件服务器主要是提供一个文件共享的功能，通过权限的设置来限制不同用户的访问需求。而FTP服务器是实现上传下载服务的功能的，一般情况下的File服务仅仅只能够在局域网内部使用，而FTP服务可以发布到公网上，作为专门的下载网站。

## 文件上传需要注意事项

1. 表单中必须添加属性 enctype="multipart/form-data"
2. action 中接收文件使用File 类型的属性接收文件实体，使用String类型的属性接收文件名信息
3. 数据库中存储的是上传后的路径
4. 前台回显时是虚拟路径+数据库存储的文件路径
5. 如果修改时不修改图片，只需要在action中判断file属性是否为空

## 什么是转换流？
## 什么是管道流？
## 什么是打印流？
## 什么是序列流？
## 什么是RandomAccessFile？

[//]: # (todo)