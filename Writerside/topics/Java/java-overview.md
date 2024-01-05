# java概述

## java的架构 {id="java_5"}
<img src="java-platform.jpg" alt=""/>

## 什么是java？ {id="java_3"}
1. 是SUN(Stanford University Network，斯坦福大学网络公司)1995年推出的一门高级编程语言。
2. 是一种面向Internet的编程语言。
3. 随着Java技术在web方面的不断成熟，已经成为Web应用程序的首选开发语言。
4. 是简单易学，完全面向对象，安全可靠，与平台无关（移植性好）的编程语言。

## Java语言的技术架构 {id="java_4"}
Java5.0版本后，更名为 JAVAEE、JAVASE和JAVAME。
### J2EE(Java 2 Platform Enterprise Edition)企业版
是为开发企业环境下的应用程序提供的一套解决方案。该技术体系中包含的技术如 Servlet Jsp等，主要针对于Web应用程序开发。
### J2SEJava 2 Platform Standard Edition）标准版
是为开发普通桌面和商务应用程序提供的解决方案。该技术体系是其他两者的基础，可以完成一些桌面应用程序的开发。比如Java版的扫雷。
### J2ME(Java 2 Platform Micro Edition)小型版
是为开发电子消费产品和嵌入式设备提供的解决方案。该技术体系主要应用于小型电子消费类产品，如手机中的应用程序等。


## 什么jdk？
JDK(Java Development Kit Java开发工具包)。JDK是提供给Java开发人员使用的，其中包含了java的开发工具，也包括了JRE。所以安装了JDK，就不用在单独安装JRE了。其中的开发工具：编译工具(javac.exe) 打包工具(jar.exe)等

## 什么是jre？
JRE(Java Runtime Environment Java运行环境)。包括Java虚拟机(JVM Java Virtual Machine)和Java程序所需的核心类库等，如果想要运行一个开发好的Java程序，计算机中只需要安装JRE即可。

## 什么是跨平台性？
通过Java语言编写的应用程序在不同的系统平台上都可以运行。

## 跨平台性原理是什么？
只要在需要运行java应用程序的操作系统上，先安装一个Java虚拟机(JVM Java Virtual Machine)即可。 由JVM来负责Java程序在该系统中的运行。

## 为什么要配置java环境变量？ {id="java_env"}
1. 配置Java环境变量是为了让计算机能够找到并正确使用Java开发工具包（JDK）中的命令和库。
2. 通过配置环境变量，你可以将Java的安装路径添加到操作系统的搜索路径中，这样你就可以在任何位置运行Java命令，而无需输入完整的路径。

## 什么是java标识符？ {id="java_ssid"}
在Java语言中，标识符是用来给类、接口、方法、变量等起名字的字符序列，好的命名规范可以提高代码的可读性和可维护性。

1. 变量名和方法名应该以小写字母开头，多个单词可以用下划线(_)分隔，例如：my_variable、my_method。
2. 类名和接口名应该以大写字母开头，多个单词可以用驼峰式命名法(CamelCase)连接，例如：MyClass、MyInterface。
3. 常量名应该全部用大写字母，多个单词可以用下划线(_)分隔，例如：MAX_VALUE。
4. 变量名和方法名不应该使用Java保留字，例如：int、class、void等。