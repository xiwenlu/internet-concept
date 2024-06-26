# mysql

1. mysql默认端口号是3306。

## mysql分页 {id="mysql_1"}

```sql
# mysql分页
# 从users表中选取第11到第20条的记录
SELECT * FROM users LIMIT 10 OFFSET 10;

# 选取第21到第30条记录
SELECT * FROM users LIMIT 10 OFFSET 20;

# 从第11条记录开始选取10条记录
SELECT * FROM users LIMIT 10, 10;

```

## mysql命令 {id="mysql_2"}
启动mysql服务器：net start mysql  
停止mysql服务器：net stop mysql  
启动mysql应用：mysql -h localhost -u root -p 3306  
退出mysql应用：quit  
查看所有数据库：show databases  
打开指定数据库：use xxx  
查看当前数据库：select database() from dual  
查看所有数据表：show tables  
查看指定表结构：desc xxx  
执行外部sql文件：source xxx

## 什么是mysql的读写分离？ {id="mysql_3"}
MySQL的读写分离是一种数据库架构技术，通过将读操作和写操作分散到不同的服务器上，以提高数据库的整体性能和可扩展性。在这种架构中，主服务器负责处理事务性查询和写操作（INSERT、UPDATE、DELETE），而从服务器只负责处理读操作（SELECT）。主服务器将数据变更同步到从服务器，从而实现数据的一致性和备份。


## 为什么要主从同步？
1. 在Web应用系统中，数据库性能是导致系统性能瓶颈最主要的原因之一。尤其是在大规模系统中，数据库集群已经成为必备的配置之一。集群的好处主要有：查询负载、数据库复制备份等。
2. 其中Master负责写操作的负载，也就是说一切写的操作都在Master上进行，而读的操作则分摊到Slave上进行。这样一来的可以大大提高读取的效率。
3. 写操作涉及到锁的问题，不管是行锁还是表锁还是块锁，都是比较降低系统执行效率的事情。我们这样的分离是把写操作集中在一个节点上，而读操作其他的N个节点上进行，从另一个方面有效的提高了读的效率，保证了系统的高可用性。

## mysql主从同步配置步骤 {id="mysql_4"}
1. 在主服务器上，需要修改MySQL配置文件（如my.cnf或my.ini），指定server-id和启用二进制日志（log-bin）。这些设置允许主服务器记录数据变更并发送给从服务器。
2. 重启MySQL服务，确保配置更改生效。
3. 创建一个专门用于复制的用户，并授权该用户复制权限。
4. 查看主服务器的二进制日志状态，记录下File和Position的值，稍后在从服务器上配置时会用到。
5. 在从服务器上，同样修改MySQL配置文件，指定server-id，启用中继日志（relay-log）和二进制日志（log-bin），并设置只读模式以防止写入操作。
6. 重启MySQL服务，确保配置更改生效。
7. 在从服务器上配置复制参数，指定主服务器的IP地址、复制用户的凭据以及之前记录的主服务器二进制日志信息。
8. 启动从服务器上的复制进程。
9. 检查从服务器的复制状态，确保Slave_IO_Running和Slave_SQL_Running的值都是"Yes"，表示主从同步正常进行。

## mysql主从同步如何降低延迟？ {id="mysql_5"}
1. 主库和同库尽可能在同一个局域网内，交换机网卡采用千兆网卡。
2. 主数据库更新完成之后产生的操作日志不是瞬间产生的，我们可以通过设置sync(sen ke)_binlog=1, 让它瞬间产生磁盘日志，（n=1指主数据库只要操作一次，就产生一次磁盘日志，n=10，就是操作10次，产生一次），从数据库可以依赖磁盘日志，瞬间产生同步，可以达到减低延迟的效果。
3. 设置主库和从库读取日志失败之后，及时重新建立连接，延迟缩短。


## mysql每个引擎各自的特点是什么？

| 功能     | myisam | memory | innodb | archive |
|--------|--------|--------|--------|---------| 
| 储存限制   | 256tb  | ram    | 64tb   | none    |
| 支持事务   | ❌      | ❌      | √      | ❌       |
| 支持全文索引 | √      | ❌      | ❌      | ❌       |
| 支持数索引  | √      | √      | √      | ❌       |
| 支持哈希索引 | ❌      | √      | ❌      | ❌       |
| 支持数据缓存 | ❌      | n/a    | √      | ❌       |
| 支持外键   | ❌      | ❌      | √      | ❌       |

## mysql引擎MyISAM和InnoDB的区别?
1. MyISAM不支持事务，每次查询具有原子性，由于较高的插入和查询记录的效率，主要用于插入和查询；InnoDB支持事务，具有事务提交、回滚和崩溃修复能力的事务安全能力，实现并发控制。
2. MyISAM只支持表锁，InnoDB支持表锁、行锁、行锁大幅度提高了多用户并发操作的性能。但是InnoDB的行锁，只是在WHERE的主键是有效的，非主键的WHERE都会锁全表的。
3. MyISAM不支持外键，InnoDB支持外键。

总的来说，MyISAM和InnoDB各有优劣，各有各的使用环境，但是InnoDB的设计目标是用来处理大容量的数据库系统的，我个人觉得使用InnoDB可以应对更为复杂的情况，特别是对并发的处理要比MyISAM高效。


## mysql中like查询为什么不区分大小写？
在MySQL中，字符集latin1、utf8_general_ci和utf8mb4_general_ci在LIKE查询时不区分大小写的。在大型数据集上，不区分大小写的排序规则可能会影响查询性能。

```sql
# character_set_server对应的值就是mysql字符集
SHOW VARIABLES LIKE 'character_set%';
# 区分大小写
select * from table_name where name  COLLATE utf8_bin like concat('%','FILE','%') ;

```

## MySQL的SET数据类型
MySQL的SET数据类型是一种特殊的字符串类型，可以包含预定义的字符串值。每个值都有一个索引，从1开始。你可以使用FIND_IN_SET函数来查询SET类型字段是否包含特定的值。在内部，MySQL使用位向量来表示SET类型的值。这种数据类型从MySQL 4.1版本开始就已经存在。

优点：
1. SET类型可以高效地存储和检索一组预定义的值。在内部，MySQL使用一个位向量来表示SET类型的值，这使得检索和比较操作非常快速。
2. SET类型可以存储多个值，但只占用一个字段的空间，这使得它在存储和管理一组相关的布尔值时非常有用。

缺点：
1. SET类型的最大限制是64个成员。如果你需要存储更多的值，你将需要使用其他数据类型或者设计一个更复杂的数据库结构。
2. SET类型的值在创建表时就需要定义，这意味着如果你需要添加、删除或修改可能的值，你将需要修改表结构。
3. SET类型的值是区分大小写的，这可能会导致一些意想不到的结果。例如，'Value1'和'value1'会被视为两个不同的值。
4. SET类型不是SQL标准的一部分，这意味着使用SET类型可能会降低你的数据库的可移植性。