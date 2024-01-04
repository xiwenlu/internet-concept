# io流

File java.io.File;
//代表了电脑中的一个文件或者文件夹
//实例化对象
File file = new File("这里是文件或者文件夹的路径");//可以是相对路径或绝对路径
file.exists();//判断这个文件或文件夹是否存在,返回布尔类型
file.isFile();//判断是否是一个文件,返回布尔类型
file.isDirectory();//判断是否是一个文件夹,返回布尔类型
file.delete();//删除文件或文件夹,返回布尔类型,代表是否删除成功
file.getAbsolutePath();//返回文件或文件夹的绝对路径
file.getName();//返回文件名或者是文件夹名
file.getParent();//返回父目录
file.getPath();//返回路径
file.isHidden();//判断是否是一个隐藏文件,返回布尔类型
file.createNewFile();//根据路径创建一个文件
file.mkdir();//根据路径创建一个文件夹
I/O流的分类
根据数据流向的不同
输入流
从外部文件将数据读取到程序中(从硬盘中将数据读取内存)
输出流
从程序将数据写出到外部文件中(从内存中将数据写出到硬盘)
根据文件的类型的不同,和存储单位的不同
字节流
是字节为单位,一个字节一个字节读写数据
一般情况下用来读写 图片 视频等类型的文件
字符流
是字符为单位读写数据,一个字符一个字符的读写数据
用来读写以字符为单位存储的文件
I/O流的结构
InputStream java.io.InputStream; 抽象类 字节输入流
FileInputStream java.io.FileInputStream; 字节输入流实现类
OutputStream java.io.OutputStream 抽象类 字节输出流
FileOutputStream java.io.FileOutputStream 字节输出流实现类
Reader java.io.Reader; 抽象类 字符输入流
FileReader java.io.FileReader; 字符流输入实现类
Writer java.io.Writer; 抽象类 字符输出流
FileWriter java.io.FileWriter; 字符流输出实现类
用IO流操作文件的步骤
1.实例化对象
2.读/写操作
3.关闭流
4.处理异常
FileInputStream字节输入流的用法
//用多态的形式实例化对象,参数为要读取的文件路径
InputStream in = null;//为了让in对象的作用域更大,所以定义在了最外面
try{
//创建InputStream的子类,参数为要读取的文件路径
//文件路径中的斜杠可以用斜杠(/),如果用反斜杠(\)记得转义,写成\\
in = new FileInputStream("D:/test.txt");//因为创建对象时会抛异常,所以需要进行捕获
int read = in.read();//调用输入流的read方法读取一个字节,如果读取中文会出现乱码
//循环读取文件中的内容
while(read!= (-1)){//当读取到的是-1时,说明文件读取完了
System.out.print((char)read);//将字节转换为字符输出
read = in.read();//读取下一个字节
}
}catch(Exception e){
//这里处理异常
}finally{
if(in!=null){//如果对象不为空
try{
in.close();//关闭流,释放资源
}catch(Exception e){
//这里处理异常
}
}
}
FileOutputStream字节输出流的用法
//用多态的形式实例化对象,参数为要写出的文件路径
//文件路径中的斜杠可以用斜杠(/),如果用反斜杠(\)记得转义,写成\\
OutputStream out = null;//为了让out对象的作用域更大,所以定义在了最外面
try{
//创建OutputStream的子类,参数为要写出的文件路径
//文件路径中的斜杠可以用斜杠(/),如果用反斜杠(\)记得转义,写成\\
out = new FileOutputStream("D:/test.txt");//因为创建对象时会抛异常,所以需要进行捕获
//out = new FileOutputStream("D:/test.txt",true);//可以往这个文件中追加内容
String str = "这里为要写出的内容";
//将字符串转化成byte数组才可以写出
out.write(str.getBytes());//write方法参数需要byte数组,调用String对象的getBytes方法可以获取byte数组
}catch(Exception e){
//这里处理异常
}finally{
if(out!=null){//如果对象不为空
try{
out.close();//关闭连接,释放资源
}catch(Exception e){
//这里处理异常
}
}
}
FileWriter字符输出流的用法
//用多态的形式实例化对象,参数为要写出的文件路径
//文件路径中的斜杠可以用斜杠(/),如果用反斜杠(\)记得转义,写成\\
Writer out = null;//为了让out对象的作用域更大,所以定义在了最外面
try{
//创建Writer的子类,参数为要写出的文件路径
//文件路径中的斜杠可以用斜杠(/),如果用反斜杠(\)记得转义,写成\\
out = new FileWriter("D:/test.txt");//因为创建对象时会抛异常,所以需要进行捕获
//out = new FileWriter("D:/test.txt",true);//可以往这个文件中追加内容
String str = "这里为要写出的内容";
out.write(str);//字符输出流可以直接写出字符串,不需要转换成byte数组
}catch(Exception e){
//这里处理异常
}finally{
if(out!=null){//如果对象不为空
try{
out.close();//关闭连接,释放资源
}catch(Exception e){
//这里处理异常
}
}
}
FileReader字符输入流的用法,需要注意的是读取的文件编码格式要是UTF-8才可以,否则会出现乱码
//用多态的形式实例化对象,参数为要读取的文件路径
Reader in = null;//为了让in对象的作用域更大,所以定义在了最外面
try{
//创建Reader的子类,参数为要读取的文件路径
//文件路径中的斜杠可以用斜杠(/),如果用反斜杠(\)记得转义,写成\\
in = new FileReader("D:/test.txt");//因为创建对象时会抛异常,所以需要进行捕获
int read = in.read();//调用输入流的read方法读取一个字节,可以读取中文
//循环读取文件中的内容
while(read!= (-1)){//当读取到的是-1时,说明文件读取完了
System.out.print((char)read);//将字节转换为字符输出
read = in.read();//读取下一个字节
}
}catch(Exception e){
//这里处理异常
}finally{
if(in!=null){//如果对象不为空
try{
in.close();//关闭流,释放资源
}catch(Exception e){
//这里处理异常
}
}
}
BufferedReader java.io.BufferedReader; 字符缓冲输入流
Reader in = null;//定义基础流
BufferedReader br = null;//定义包装流,缓冲流
try {
in = new FileReader("test2.txt");//先创建基础流对象
br = new BufferedReader(in);//将基础流对象当参数传给缓冲流
String str = br.readLine();//可以读取一行字符串内容
while(str!=null){//如果读取完了内容会返回null,所以不为null就继续读取内容
System.out.println(str);
str = br.readLine();//读下一行
}
} catch (Exception e) {
}finally{
try {
if(br!=null){//先关闭外层流,也就是缓冲流
br.close();
}
if(in!=null){//在关闭里面的流,也就是基础流
in.close();
}
} catch (Exception e2) {
}
}
BufferedWriter java.io.BufferedWriter; 字符缓冲输出流
Writer out = null;//定义基础流
BufferedWriter bw = null;//定义包装流,缓冲流
try {
out = new FileWriter("test2.txt");//先创建基础流对象
bw = new BufferedWriter(out);//将基础流对象当参数传给缓冲流
String str = "金科教育";//定义要输出的内容
bw.write(str);//写出要输出的内容
bw.newLine();//调用这个方法可以将输出的内容进行换行
bw.write("万薪就业");写出要输出的内容
bw.flush();//记得清空缓冲区内容
} catch (Exception e) {
}finally{
try {
if(bw!=null){//先关闭外层流,也就是缓冲流
bw.close();
}
if(out!=null){//在关闭里面的流,也就是基础流
out.close();
}
} catch (Exception e2) {
}
}
BufferedInputStream java.io.BufferedInputStream; 字节缓冲输入流
InputStream in = null;//定义基础流
BufferedInputStream bis = null;//定义包装流,缓冲流
try {
in = new FileInputStream("test1.txt");//先创建基础流对象
bis = new BufferedInputStream(in);//将基础流对象当参数传给缓冲流
byte[] bs = new byte[1024];//定义一个要读取的字节大小
//根据传递的字节数组大小,一次性读取数组长度的内容,读取的内容将会存在字节数组中,而不是之前返回的数据
int read = bis.read(bs);
while(read!= -1){//如果是-1则代表读取完毕
String str = new String(bs);//通过String的构造函数,将字节数组转换成字符串
System.out.println(str);
read = bis.read(bs);//继续根据字节数组的大小,一次性读取数组长度的内容
}
} catch (FileNotFoundException e) {
e.printStackTrace();
} catch (IOException e) {
e.printStackTrace();
}finally{
try {
if(bis!=null){//先关闭外层流,也就是缓冲流
bis.close();
}
if(in!=null){//在关闭里面的流,也就是基础流
in.close();
}
} catch (IOException e) {
e.printStackTrace();
}
}
BufferedOutputStream java.io.BufferedOutputStream; 字节缓冲输出流
OutputStream out = null;//定义基础流
BufferedOutputStream bos = null;//定义包装流,缓冲流
try {
out = new FileOutputStream("test1.txt");//先创建基础流对象
bos = new BufferedOutputStream(out);//将基础流对象当参数传给缓冲流
String str = "jinkejiaoyu";
bos.write(str.getBytes());//操作缓冲流写出对象
bos.flush();//将缓冲区域中的内容写出到文件中
} catch (FileNotFoundException e) {
e.printStackTrace();
} catch (IOException e) {
e.printStackTrace();
}finally{
try {
if(bos!=null){//先关闭外层流,也就是缓冲流
bos.close();
}
if(out!=null){//在关闭里面的流,也就是基础流
out.close();
}
} catch (IOException e) {
e.printStackTrace();
}
}