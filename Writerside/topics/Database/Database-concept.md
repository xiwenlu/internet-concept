# 数据库概念

## 什么是sql？ {id="sql_4"}
SQL（Structured Query Language）是用于管理关系数据库的标准编程语言。通过SQL，用户可以对数据库进行查询、插入、更新和删除数据，以及定义和管理数据库结构等操作。

## sql语句中的语言类别 {id="sql_3"}

### data definition language 数据定义语言（DDL）
用于改变数据库结构，包括创建、修改和删除数据库对象，包括create（创建）、alter（修改）和drop（删除）等命令操作。

### data manipulation language 数据操纵语言（DML）
用户通过它可以实现对数据库的基本操作，包括insert（插入）、delete（删除）、update（更改）和select（查询）等命令操作。 

   
### transaction control language 事务控制语言（TCL）
包括commit（提交）命令、save point（保存点）命令和 rollback（回滚）等命令操作。 
   
### data control language 数据控制语言（DCL）
包括grant（授权）和 revoke（撤销权限）等命令操作。 
   
### data query language 数据查询语言 (DQL)
包括基本查询语句、order by子句和 group by子句等。


## sql中表的来源 {id="sql_5"}
1. 用户自己创建 
2. oracle内部自带的

## 主键（primary key）
1. 主关键字是被挑选出来作表的行的唯一标识的候选关键字。一个表只有一个主关键字。主关键字又可以称为主键。
2. 主键可以由一个字段，也可以由多个字段组成，分别称为单字段主键或多字段主键。又称主码。并且它可以唯一确定表中的一行数据，或者可以唯一确定一个实体。

### 主键的特点
1. 对表中的字段限制唯一且不可为空一般我们会将主键设置在id上，我们可以通过主键快速的找到我们想要的指定的数据（类似于我们的身份证号）
2. 一个表只能有一个主键，可以是单字段主键或多字段主键

## 外键（foreign key）
约束内表的数据的更新，从定义外键时可以发现 外键是和主键表联系，数据类型要长度统一，(存储大小)要统一。这样在更新数据的时候会保持一致性。
1. Not null 非空约束
2. Unique 唯一约束


## 序列（sequence）
序列是oracle提供的用于产生一系列唯数一字的数据库对象。自动提供唯一的数值，共享对象，主要用于提供主键值。将序列值装入内存可以提高访问效率。
```SQL
create sequence seq_stu_id
increment by 1
minvalue 0
nomaxvalue
start with 0
cache 5
nocycle
order
```

## 什么是DBA？
1. DBA指的是数据库管理员（Database Administrator），是从事管理和维护数据库管理系统(DBMS)的相关工作人员的统称，属于运维工程师的一个分支，主要负责业务数据库从设计、测试到部署交付的全生命周期管理。
2. DBA的具体职责包括监控、管理、维护、调优、设计等操作，确保数据库服务7*24小时的稳定高效运转。

## 什么是ORDBMS？
对象关系型数据库管理系统（ORDBMS）是一种数据库管理系统，它既具备关系数据库的功能，同时又支持面向对象的特征。这种数据库管理系统结合了关系数据库技术和面向对象数据库技术的特性。
1. O: Object对象
2. R：Relation关系
3. DB：DataBase数据库
4. M :Manage 管理
5. S :System 系统

## 什么是数据库分区？
对外只展示一张表，但是表内部分区到不同的磁盘上，只需要其中一部分数据的时候可直接映射相应的区进行查找，避免了全表扫描，提升了查找，插入数据的性能，一般是数据库层面实现的，分区可分为水平分区和垂直分区，通常水平分区用的比较多，算法有按照某个字段的大小等，某个字段的hash分等等。

## 什么是sql注入？ {id="sql_1"}
通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。

## 怎么防止sql注入
1. 永远不要信任用户的输入。对用户的输入进行校验，可以通过正则表达式或限制长度对单引号和双"-"进行转换等。
2. 永远不要使用动态拼装sql，可以使用参数化的sql或者直接使用存储过程进行数据查询存取。
3. 永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接。
4. 不要把机密信息直接存放，加密或者hash掉密码和敏感的信息。
5. 应用的异常信息应该给出尽可能少的提示，最好使用自定义的错误信息对原始错误信息进行包装。
6. sql注入的检测方法一般采取辅助软件或网站平台来检测，软件一般采用sql注入检测工具jsky，网站平台就有亿思网站安全平台检测工具。MDCSOFT SCAN等。采用MDCSOFT-IPS可以有效的防御SQL注入，XSS攻击等。

## 左连接、右连接、内连接和全外连接的区别 {id="sql_2"}
1. 左连接显示左边全部的数据和右边与左边相同的数据
2. 右连接显示右边全部的数据和左边与右边相同的数据
3. 内连接是只显示满足条件的数据
4. 全外连接是在等值连接的基础上将左表和右表的未匹配数据都加上

```SQL

# 左连接
select * from 表一 left join 表二 on 连接条件 where。。。
select * from 表一 left outer join 表二 on 连接条件 where。。。

# 右连接
select * from 表一 right join 表二 on 连接条件 where。。。
select * from 表一 right outer join 表二 on 连接条件 where。。。

# 内连接(等值连接)
select * from 表一 inner join 表二 on 连接条件 where。。。

# 全外连接
select * from 表一 full join 表二on连接条件 where。。。


```

## row_number()、rank()和dense_rank()区别
1. ROW_NUMBER()为结果集的每一行分配一个唯一的整数。即使两行的排序值相同，也会为它们分配不同的行号。
2. RANK()为结果集的每一行分配一个整数。如果两行的排序值相同，它们将获得相同的排名。下一个行将跳过相应的数量（例如，如果两行获得排名 2，下一行将获得排名 4）。
3. DENSE_RANK()与 RANK() 类似，但不会跳过任何排名。也就是说，如果有两行获得排名 2，下一行将获得排名 3。
```SQL
SELECT name, score, ROW_NUMBER() OVER (ORDER BY score DESC) as row_num  FROM students;
SELECT name, score, RANK() OVER (ORDER BY score DESC) as rank  FROM students; 
SELECT name, score, DENSE_RANK() OVER (ORDER BY score DESC) as dense_rank  FROM students;

```

## 索引
```sql
# 创建表
create table table_name(
   id int not null auto_increment primary key,
   name varchar(32) not null,
   email varchar(64) not null,
   extra text,
   index ix_name (name)
);

# 创建索引
# 对于创建索引时如果是BLOB 和 TEXT 类型，必须指定length。
create index index_name on table_name(column_name);
create index ix_extra on table_name(extra(32));

# 删除索引
drop index_name on table_name;

# 查看索引 
show index from table_name;

# 使用索引。like '%xx'，避免%_写在开头
select * from table_name where name like '%n';
```

### 优点
1. 创建唯一索引，保证数据库表中每一行数据的唯一性
2. 大大加快数据的检索速度，这也是创建索引的最主要原因
3. 减少磁盘IO(像字典一样可以直接定位)

### 缺点
1. 创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加
2. 索引需要占用额外的物理空间
3. 当对表中的数据进行增加、删除和修改的时候，索引也要动态维护，降低了数据的维护速度

## 存储过程
存储过程是一个SQL语句集合，类似函数，需要主动调用，且参数有三类。
1. in 仅用于传入参数用
2. out 仅用于返回值用
3. inout 既可以传入又可以当作返回值

```sql
# 无参数存储过程
delimiter //
create procedure p1()
BEGIN
    select * from t1;
END //
delimiter ;

#执行存储过程
call p1()

# 有参数存储过程
delimiter \\ #结尾分号改为\\
create procedure p1(ini1 int,ini2 int,inout i3 int,out r1 int)
BEGIN
DECLARE temp1 int;#创建申明局部变量
DECLARE temp2 int default 0;
set temp1= 1;
set r1= i1 + i2 + temp1 +temp2;
set i3= i3 + 100;
end\\
delimiter ;

# 执行存储过程
DECLARE @t1 INT default 3;
DECLARE @t2 INT;
CALL p1 (1, 2,@t1, @t2);
SELECT @t1,@t2;
```

### 优点
1. 简化了复杂的业务逻辑，根据需要可以重复使用
2. 屏蔽了底层细节，不暴露表结构即可完成操作
3. 降低网络通信量，多条语句可以封装成一个存储过程来执行
4. 设置访问权限来提高安全性
5. 提高执行效率，因为它是预编译的以及存储在数据库中

### 缺点
1. 可移植性差，相同的存储过程并不能跨多个数据库操作
2. 大量使用存储过程后，首先会造成服务器压力大，而且维护难度逐渐增加


## 函数
函数，与存储过程不同的是有return值。 自定义函数可以接受参数并返回一个值，允许您编写自己的逻辑并在查询中使用它。
```SQL
CREATE FUNCTION add_numbers(a INT, b INT)  
RETURNS INT  
BEGIN  
    RETURN a + b;  
END;
SELECT add_numbers(3, 5); -- 返回 8
```

### 优点
1. 函数允许标准组件式编程，提高了SQL语句的重用性、共享性和可移植性。
2. 函数可以被作为一种安全机制来利用。
3. 函数能够实现较快的执行速度，能够减少网络流量。

### 缺点
1. 函数的编写比单句SQL语句复杂。
2. 在编写函数时，需要创建这些数据库对象的权限。

## 窗口函数
### partition by
用于将数据分割成不同的分区，并在每个分区上独立应用窗口函数。通过 PARTITION BY 子句，你可以指定一个或多个列，将数据按照这些列的值进行分区。
```SQL
# 假设你有一个名为 employees 的表，其中包含员工的工资和部门信息。你可以使用 PARTITION BY 子句按部门对员工进行分区，并在每个分区上计算每个员工的相对工资排名。
SELECT department, salary, RANK() OVER (PARTITION BY department ORDER BY salary DESC) as salary_rank  FROM employees;
```


