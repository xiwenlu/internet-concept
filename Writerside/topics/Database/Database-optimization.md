# 数据库优化

## SQL优化
1. 不要使用*号 select * from xxx，因为使用*号的话数据库会先去查询一下表结构获得匹配的字段后再进行一次查询
2. SQL语句使用大写 select a,b,c from xxx，几乎所有的数据库在执行SQL的时候都会先把执行的SQL语句转换成大写，如果直接使用大写字母可省略这个步骤
3. 不使用子查询
```sql   
select * from xxx where id=（select id from b where name=’aaa’）
```
因为SQL执行时先查外表再匹配内表，而不是先查内表（子查询），当外表的数据很大时，查询速度会非常慢。应采用join方式进行连接查询
4. 避免规避索引的操作，应尽量避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描，尽量避免在 where 子句中使用 or 来连接条件，否则将导致引擎放弃使用索引而进行全表扫描，如：
```sql    select id from t where num=10 or num=20```

可以这样查询：
```sql  
select id from t where num=10
union all
select id from t where num=20
```

in 和 not in 也要慎用，否则会导致全表扫描，如：
```sql 
select id from t where num in(1,2,3)
```

对于连续的数值，能用 between 就不要用 in 了：
```sql 
select id from t where num between 1 and 3
```
尽量避免在 where 子句中对字段进行表达式操作，尽量避免在where子句中对字段进行函数操作，用IN来替换OR，不要写一些没有意义的查询，如需要生成一个空表结构，很多时候用 exists 代替 in 是一个好的选择等等
5. 避免频繁创建和删除临时表，以减少系统表资源的消耗，等等      
   
## 数据库优化
数据库优化法则归纳为5个层次：
1. 减少数据访问（减少磁盘访问），具体的实现是        
   (1) 创建并使用正确的索引       
   (2) 只通过索引访问数据        
   (3) 优化SQL的执行     
2. 返回更少数据（减少网络传输或磁盘访问）      
   (1) 数据分页处理           
   (2) 只返回需要的字段         
3. 减少交互次数（减少网络传输）       
   (1) 使用缓存     
   (2) 使用存储过程           
   (3) 优化业务逻辑           
   (4) 使用游标
4. 减少服务器CPU开销（减少CPU及内存开销）           
   (1) 使用绑定变量           
   (2) 合理使用排序           
   (3) 减少比较操作           
   (4) 大量复杂的运算在客户端处理            
5. 利用更多资源（增加资源）         
   (1) 客户端多进行并行访问           
   (2) 数据库并行处理      
   

## SQL优化： {id="sql_1"}
1. sql的执行顺序：from 表名 where 条件 ，执行顺序是从后往前，where条件后面的语句尽可能缩短where 数据执行的范围。先group by  后order by  select 查询
2. 避免过多的联查，设计合理的表关系
3. 遵守常见sql规范        
3.1尽可能减少*       
3.2.尽量多使用commit如对大数据量的分段批量提交释放了资源，减轻了服务器压力      
3.3.尽量保持每次查询的sql语句字段用大写，oracle会先把sql解析成大写在执行        
3.4.用NOT EXISTS 或（外链接+判断为空）方案 替换 NOT IN 操作符，        
3.5合理使用exists 和not exists ，not exists 的效率高于exists），根据业务需求按字段进行查询        
4. 如果表字段过多，经常展示的字段较少，对表进行纵切割。（从表的中间进行切割成两张表）
5. 如果单个表的数量过大，利用业务逻辑采用横切割，例如腾讯qq采用位数分表，注册用手机号前三位来分表，
6. 适度冗余减少关联查询
7. 采用读写分离机制降低单个数据库压力
8. 适当建立索引（数据超过5万条，十万条，才有作用，索引分为单个索引和复合索引，避免在索引列上使用计算和函数，这样索引就不能使用）提高查询效率。           
8.1 有索引的字段避免出现not，<>，!= 使用空值，使用函数，出现数据转换。避免对字段进行函数或表达式操作，这样有可能造成索引失效；        
8.2 索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有必要）。         
9. 用 TRUNCATE 代替DELETE TRUNCATE 不记录日志，DELETE记录日志，所以TRUNCATE 要快于DELETE 但是一旦使用TRUNCATE 删除就不能进行恢复，TRUNCATE 是删除整张表的数据不能加where条件。mysql,sqlserver中如果id为自增类型，那么如果用TRUNCATE 删除，则id字段再插入数据时从1开始，如果delete删除的话，则从删除之前的id的值继续增长。
10. 尽量避免使用临时表，减少与数据库的交互。
11．尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这会降低查询和连接的性能，并会增加存储开销。这是因为引擎在处理查询和连 接时会逐个比较字符串中每一个字符，而对于数字型而言只需要比较一次就够了。



## 数据库优化：
数据库优化吧我觉应该从硬盘、内存和网络带宽考虑，提高硬盘的读写速度，增大带宽提高吞吐量，增大服务器内存，可以采用读写分离，降低单台数据库的访问压力，查询的时候控制数据量的大小，返回更少数据，减少交互次数，减少cpu及内存的开销，      
sql优化：如果一个表中数据量过大我们可以采用横切割，如果一个表中字段过多，我们可以采用纵切割，适度冗余减少表关联查询，避免过多的联查，设计合理的表关系，适当建立索引，但是不能每个字段都建立索引，因为索引也要占用一定的物理存储空间，而且索引也需要动态维护，增加索引虽然可以提高查询速度，但是如果索引过多就会降低增删改的速度，写sql的时候也要注意，尽可能不要写一些让索引失效的sql，例如：索引的字段不能为空，不能进项模糊搜索，不能进项逻辑运算，不能使用函数，给索引查询的值应是已知数据，不能是未知字段值。也可以使用force(fou si)强制走索引，咱们常用的索引包括，单个索引、唯一索引、复合索引，单个索引就是创建的索引中只包含一个字段，复合索引是创建的一个索引中包含多个字段，      
创建索引：
```sql
create index index_name on table_name(column_name)
```

## jvm优化、tomcat优化、sql优化、数据库优化、代码优化
代码优化：    
1. 及时关闭流    
Java编程过程中，进行数据库连接、I/O流操作时务必小心，在使用完毕后，及时关闭以释放资源。因为对这些大对象的操作会造成系统大的开销，稍有不慎，将会导致严重的后果。
2. 尽量减少对变量的重复计算
明确一个概念，对方法的调用，即使方法中只有一句语句，也是有消耗的，包括创建栈帧、调用方法时保护现场、调用方法完毕时恢复现场等。所以例如下面的操作：
```java
for (int i = 0; i < list.size(); i++){...}
```
建议替换为：
```java
for (int i = 0, int length = list.size(); i < length; i++){...}
```

这样，在list.size()很大的时候，就减少了很多的消耗      
3. for循环里减少对数据库的交互      
4. 不要在循环中使用try…catch…，应该把其放在最外层     
5. 尽量使用HashMap、ArrayList、StringBuilder，除非线程安全需要，否则不推荐使用Hashtable、Vector、StringBuffer，后三者由于使用同步机制而导致了性能开销    
6. 将重复的代码封装起来，尽量避免重复代码的出现        

## 如何应对数据的高并发，大量的数据计算
1. 创建索引
2. 数据库读写分离，两个数据库，一个作为写，一个作为读
3. 外键去掉
4. django中orm表性能相关的
   select_related：一对多使用，查询主动做连表
   prefetch_related：多对多或者一对多的时候使用，不做连表，做多次查询


## Sql 优化,数据库优化。 至少10条。
`SQL优化：`     
1. sql的执行顺序：from 表名 where 条件 ，执行顺序是从后往前，where条件后面的语句尽可能缩短where 数据执行的范围。先group by  后order by  select 查询
2. 避免过多的联查，设计合理的表关系
3. 遵守常见sql规范
   1. 尽可能减少*
   2. 尽量多使用commit如对大数据量的分段批量提交释放了资源，减轻了服务器压力
   3. 尽量保持每次查询的sql语句字段用大写，oracle会先把sql解析成大写在执行
   4. 用NOT EXISTS 或（外链接+判断为空）方案 替换 NOT IN 操作符，
   5. 合理使用exists 和not exists ，not exists 的效率高于exists）， 根据业务需求按字段进行查询
4. 如果表字段过多，经常展示的字段较少，对表进行纵切割。（从表的中间进行切割成两张表）
5. 如果单个表的数量过大，利用业务逻辑采用横切割，例如腾讯qq采用位数分表，注册用手机号前三位来分表，
6. 适度冗余减少关联查询
7. 采用读写分离机制降低单个数据库压力
8. 适当建立索引（数据超过5万条，十万条，才有作用，索引分为单个索引和复合索引，避免在索引列上使用计算和函数，这样索引就不能使用）提高查询效率。
   1. 有索引的字段避免出现not，<>，!= 使用空值，使用函数，出现数据转换。避免对字段进行函数或表达式操作，这样有可能造成索引失效；
   2. 索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有必要）。
9. 用 TRUNCATE 代替DELETE    TRUNCAT不记录日志，DELETE记录日志，所以TRUNCATE 要快于DELETE 但是一旦使用TRUNCATE 删除就不能进行恢复，TRUNCATE 是删除整张表的数据不能加where条件。mysql,sqlserver中如果id为自增类型，那么如果用TRUNCATE 删除，则id字段再插入数据时从1开始，如果delete删除的话，则从删除之前的id的值继续增长。
10. 尽量避免使用临时表，减少与数据库的交互。
11. 尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这会降低查询和连接的性能，并会增加存储开销。这是因为引擎在处理查询和连 接时会逐个比较字符串中每一个字符，而对于数字型而言只需要比较一次就够了。