# linux

## linux简介 {id="linux_1"}
Linux是一套免费使用和自由传播的类Unix操作系统，是一个基于POSIX和UNIX的多用户、多任务、支持多线程和多CPU的操作系统。它能运行主要的UNIX工具软件、应用程序和网络协议。它支持32位和64位硬件。Linux继承了Unix以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。

- rpm是linux下一种软件安装包的后缀名，等同于windows中的exe。
- rpm是一个命令，用来进行和软件安装，卸载和查找相关的操作。linux下没有盘符的概念，它是通过相关的目录来管理资源的。
- 我们通常在/home创建文件夹来存放需要安装的软件
- tab键自动补全
- jdk默认安装在/usr/java中
- 针对tomcat的安装，则直接解压缩后便可使用


## linux添加java环境变量
1. 打开终端，输入*sudo vim /etc/profile*在最后一行添加以下内容
```Text
export JAVA_HOME=/usr/java/jdk1.7.0_71      
export PATH=$JAVA_HOME/bin:$PATH        
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```
2. 终端输入*source /etc/profile*使配置文件立即生效
3. 终端输入*echo $JAVA_HOME*验证是否配置成功，如果输出 JDK 的安装路径，则说明配置成功。


## linux常用命令
```Shell
# get请求接口
# -H 添加请求头
curl -H "Authorization: e897b4d9c64b42b681258dd607d949ed" \
https://www.baidu.com/api/queryById?id=5

# 把文件解压到指定的目录下,需要用到-d参数，将test压缩包解压到source目录下
unzip test.zip -d root/source/

# 将/root/source这个目录下所有文件和文件夹打包为当前目录下的 test.zip：
zip -q -r test.zip /root/source

tar -xvf  file.tar        # 解压tar包
tar -xzvf  file.tar.gz    # 解压tar.gz包
tar -xjvf  file.tar.bz2   # 解压tar.bz2包
tar -xZvf  file.tar.Z     # 解压tar.Z包

# 杀死所有进程
ps -ef | grep java | grep -v grep | awk '{print $2}' | xargs kill -9

# 查看日志
tail -100f catalina.out

# 登录linux
ssh root@www.baidu.com

# 上传
scp -r /Users/xiwenlu/nginx/nginx.conf root@www.baidu.com:/home/nginx/conf/

# 下载
scp root@www.baidu.com:/home/nginx/conf/nginx.conf /Users/xiwenlu/nginx


```
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
    <tr>
        <td>命令</td>
        <td>描述</td>
    </tr>
    <tr>
        <td>ifconfig</td>
        <td>查看ip地址</td>
    </tr>
    <tr>
        <td>java -version</td>
        <td>查看jdk的版本</td>
    </tr>
    <tr>
        <td>rpm -qa | grep 软件的名称</td>
        <td>查找和指定名称相关的软件</td>
    </tr>
    <tr>
        <td>rpm -e --nodeps 软件名称</td>
        <td>卸载指定的软件</td>
    </tr>
    <tr>
        <td>rpm -ivh 软件名称</td>
        <td>安装指定的软件</td>
    </tr>
    <tr>
        <td>uname -a</td>
        <td>查看linux系统的计算机名，操作的位数和版本号</td>
    </tr>
    <tr>
        <td>ll</td>
        <td>用来查看当前目录下的所有文件资源。</td>
    </tr>
    <tr>
        <td>mkdir 目录名</td>
        <td>创建文件夹</td>
    </tr>
    <tr>
        <td>vi 文件名</td>
        <td>对指定的文件名进行编辑。</td>
    </tr>
    <tr>
        <td>:wq!</td>
        <td>强制保存并退出</td>
    </tr>
    <tr>
        <td>:q!</td>
        <td>强制退出</td>
    </tr>
    <tr>
        <td>pwd</td>
        <td>查看当前目录的完整路径</td>
    </tr>
    <tr>
        <td>unzip 文件名.zip</td>
        <td>解压后缀名为zip的压缩文件</td>
    </tr>
    <tr>
        <td>mv 源文件名 目标文件名</td>
        <td>重命名的作用</td>
    </tr>
    <tr>
        <td>rm -rf 文件夹名</td>
        <td>递归强制删除文件夹及其下面的所有子文件</td>
    </tr>
    <tr>
        <td>service iptables stop</td>
        <td>禁用防火墙</td>
    </tr>
    <tr>
        <td>service iptables start</td>
        <td>开启防火墙</td>
    </tr>
    <tr>
        <td>chmod +x *.sh</td>
        <td>使所有后缀名为sh的文件，拥有可执行权限</td>
    </tr>
    <tr>
        <td>./startup.sh</td>
        <td>在bin目录下，启动tomcat</td>
    </tr>
    <tr>
        <td>tail -f ../logs/catalina.out</td>
        <td>在bin目录下通过，来查看启动日志</td>
    </tr>
    <tr>
        <td>ps -ef | grep 进程名</td>
        <td>查看指定进程是否启动。</td>
    </tr>
    <tr>
        <td>kill -9 进程号</td>
        <td>强制杀死进程</td>
    </tr>
    <tr>
        <td>touch 文件名称</td>
        <td>创建文件</td>
    </tr>
    <tr>
        <td>cat 文件名称</td>
        <td>查看文件内容</td>
    </tr>
    <tr>
        <td>cd 文件夹路径</td>
        <td>跳转到指定的文件夹目录</td>
    </tr>
    <tr>
        <td>cd / </td>
        <td>跳转根目录</td>
    </tr>
    <tr>
        <td>cd ../</td>
        <td>跳转到上级目录</td>
    </tr>
    <tr>
        <td>cd ../../ </td>
        <td>:跳转到上两级目录</td>
    </tr>
    <tr>
        <td>reboot</td>
        <td>重启机器</td>
    </tr>
    <tr>
        <td>cp 源文件路径 新文件目录</td>
        <td>复制文件</td>
    </tr>
    <tr>
        <td>ping</td>
        <td>测试通讯链接</td>
    </tr>
    <tr>
        <td>clear</td>
        <td>清屏</td>
    </tr>
    <tr>
        <td>tar -cvf 文件名.tar 要压缩的文件</td>
        <td>将指定的文件打包成tar</td>
    </tr>
    <tr>
        <td>tar -xvf 文件名.tar</td>
        <td>解压后缀名为tar的文件</td>
    </tr>
    <tr>
        <td>ctrl + c</td>
        <td>退出进程 多用于 退出查看日志等</td>
    </tr>
    <tr>
        <td>/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT</td>
        <td>开放端口</td>
    </tr>
    <tr>
        <td>/etc/rc.d/init.d/iptables save</td>
        <td>保存设置</td>
    </tr>
    <tr>
        <td>/etc/init.d/iptables restart</td>
        <td>重启防火墙</td>
    </tr>
    <tr>
        <td>tar -zcvf 文件名.tar.gz 要压缩的文件</td>
        <td>将指定的文件打包压缩成tar.gz</td>
    </tr>
    <tr>
        <td>tar -zxvf 文件名.tar.gz</td>
        <td>解压缩后缀名为tar.gz文件</td>
    </tr>
    </tbody>
</table>    


