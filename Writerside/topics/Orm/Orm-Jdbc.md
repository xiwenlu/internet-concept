# orm和jdbc

## ORM简介 {id="orm_1"}
ORM：对象关系映射（Object Relational Mapping，简称ORM，或O/RM，或O/R mapping)  
用于实现面向对象编程语言里不同类型系统的数据之间的转换     
1. ORM是通过使用描述对象和数据库之间映射的元数据，将程序中的对象与关系数据库相互映射
2. ORM可以解决数据库与程序间的异构性，比如：在Java中我们使用String表示字符串，而Oracle中可使用varchar2，MySQL中可使用varchar，SQLServer可使用nvarchar。

--- 
1. 它是一种将内存中的对象保存到关系型数据库中的技术
2. 主要负责实体域对象的持久化，封装数据库访问细节
3. 提供了实现持久化层的另一种模式，采用映射元数据（XML）来描述对象-关系的映射细节，使得ORM中间件能在任何一个Java应用的业务逻辑层和数据库之间充当桥梁。

## ORM的方法论基于三个核心原则
1. 简单：以最基本的形式建模数据。
2. 传达性：数据库结构被任何人都能理解的语言文档化。
3. 精确性：基于数据模型创建正确标准化了的结构。

## jdbc的连接步骤 {id="jdbc_1"}
1. 导jar包
2. 加载驱动为数据库连接做准备：Class.forName
3. 连接数据库：DriverManger.getConnection
4. 写sql语句
5. 创建数据库操作对象：prepareStatement
6. 执行sql语句：executeUpdate/executeQuery
7. 关闭连接释放资源：.close


## jdbc如何释放资源 {id="jdbc_2"}
在finally代码块中关闭连接

## jdbc缺点
1. 持久化层缺乏弹性。一旦出现业务需求的变更，就必须修改持久化层的接口
2. 持久化层同时与域模型与关系数据库模型绑定，不管域模型还是关系数据库模型发生变化，都要修改持久化曾的相关程序代码，增加了软件的维护难度。
3. 将和数据库交互（CRUD）的代码硬编码到JDBC程序中
4. 实现见状的持久化层需要高超的开发技巧，而且编程量很大
5. 对象模型和关系模型的转换非常麻烦


## 什么ORM? {id="orm_2"}
ORM(Object Relation Mapping)对象关系映射。在java对象和关系型数据库之间建立起来的映射关系。ORM这种思想下的具体技术实现包括Hibernate、Ibatis和JPA。