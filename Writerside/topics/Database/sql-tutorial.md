# sql通用教程

数据库使用mysql

## create创建表
```SQL
create table myuser
(
    id        int auto_increment primary key,
    myname    varchar(32) null,
    age       tinyint     null
)
```

## alter修改表
```SQL
# 添加字段   
alter table myuser add sex tinyint;

# 修改表字段的长度
# 没有数据时，则可以修改数据类型，那么当该字段存在数据，则只能修改其长度
alter table myuser modify myname varchar(20);

# 对表字段myname进行重命名为username
alter table myuser change myname username varchar(20) null;

# 删除表中的sex字段
alter table myuser drop column sex;

```

## rename重命名表
```sql
alter table myuser rename to myuser_bak;
```

## drop删除表
```sql
drop table myuser_bak;

# 不可回滚，不会生成操作日志
truncate myuser_bak;
```

## 表数据复制
```sql
# 不存在另一张表时(表的备份)
create table myuser_bak as select * from myuser;
# 存在另一张表时(批量插入)
insert into myuser_bak select * from myuser;
```

## insert插入
```SQL
insert into myuser (myname,age) values('a',10);
insert into myuser values(2,'a',10);
```

## update修改

```SQL
update myuser set age = 20 where id = 1;
```

## delete删除
```SQL
delete from myuser where id = 1;
```


## dual虚拟表
dual表是Oracle数据库中的一个特殊表，主要用于查询系统变量和执行一些特殊的操作。

```SQL
# 简单的数学计算
select 3*5 from dual;
```

## select查询语句
两个sql语句之间用分号隔开。*代表所有字段，查询多个字段使用逗号隔开。
```SQL
select * from myuser;
select id,myname,age from myuser;
```

## as定义别名
定义别名可以使用as或者空格来起别名
```SQL
select myname username from myuser;
select myname as username from myuser;
```

## 字符串连接
1. 单引号用在字符串中作为界定符，双引号用在字段别名。
2. 字符串中若包含单引号则双写。
3. 字符串连接符oracle使用||，mysql使用concat()函数

```SQL
select 'abc' as "abc",
       'abc''def', 
       concat('abc','def') from dual;
```

## distinct消除冗余
```SQL
select distinct age from myuser
```

## where筛选条件
```SQL
# 查询所有年龄为10的数据
select * from myuser where age=10;
```

## is null和is not null的用法
```SQL
# 查询所有年龄为空的数据
select * from myuser where age is null;
# 查询所有年龄不为空的数据
select * from myuser where age is not null; 
```

## in包含

```SQL
select * from myuser where age in(10,11,21);
```

## and、or和between and的用法
```SQL
# 查询年龄大于10岁或者名字叫22的学生的信息
select * from myuser where age>10 or name='22' ;
# 查询年龄大于10岁并且名字叫22的学生的信息
select * from myuser where age>10 and name='22';
# 查询年龄在10岁到33岁之间的所有学生的信息（包含10岁和33岁）
select * from myuser where age between 10 and 33;
```

## like用法
1. %：表示任意个字符，包括零个
2. _：表示一个任意字符
3. like ‘%$%%’ escape ‘$’指定转义字符(指定用什么符号来转义后面的字符)

```SQL
select * from myuser where myname like '%2%';

select * from myuser where myname like '_2%';

select * from myuser where myname like '%*%%' escape '*'
```

## order by排序
升序关键字asc，默认值，可省略。降序关键字desc，不能省略。
```SQL
select * from myuser order by myname;
select * from myuser order by myname desc;
```

## group by 分组
出现在select中的字段必须要么出现在group by中，要么出现在聚合函数中。
```SQL
select age from myuser group by age;
```
