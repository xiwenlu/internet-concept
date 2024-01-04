# oracle

1. oracle默认的端口号是1521


## oracle用户管理 {id="oracle_manage"}

### oracle默认的数据库用户 {id="oracle_default_user"}
数据库中所有数据字典表和视图都存储在SYS模式中。
1. SYS用户主要用来维护系统信息和管理实例。（群主）
2. SYSTEM 是默认的系统管理员，该用户拥有Oracle管理工具使用的内部表和视图。通常通过SYSTEM用户管理数据库用户、权限和存储等。（群管理员）
3. SCOTT用户是Oracle 数据库的一个示范帐户，在数据库安装时创建。（普通用户）

### 创建新用户
要连接到Oracle数据库，就需要创建一个用户帐户每个用户都有一个默认表空间和一个临时表空间。

```SQL
# 使用system登陆或者sys登录
# 查看所有的账号名
select username from dba_users;

CREATE USER <username>;      # 创建新用户
IDENTIFIED BY <password>;    # 密码
DEFAULT TABLESPACE users;    # 默认表空间
TEMPORARY TABLESPACE temp;   # 临时表空间
QUOTA unlimited ON users;    # 用户可以在USERS表空间上无限制地使用空间
GRANT CONNECT, RESOURCE TO <username>;  # 授予了连接到数据库和获取数据库资源的权限
REVOKE 权限 FROM <username>;             # 撤销权限

# ALTER USER 命令可用于更改口令，比如修改密码
ALTER USER <username> IDENTIFIED BY <new_password>;
# DROP USER 命令用于删除用户，比如级联删除
DROP USER <username> CASCADE;

```

1. Connect: 拥有Connect权限的用户只可以登录oracle，不可以创建实体，不可以创建数据库结构。
2. Resource: 拥有Resource权限的用户只可以创建实体，不可以创建数据库结构。


## oracle数据类型 {id="oracle_datatype"}

<img src="oracle-data-type.gif" alt="foo"/>


### 字符串类型
1. CHAR 数据类型，固定字节长度2000字节
2. VARCHAR2 数据类型，可变字节长度4000字节
> [char、varchar、nchar和nvarchar的区别](#datatype_diff)

### 在字符编码不同时一个汉字所占的字节数

| 数据类型\字符集  |   gbk   |  utf-8  |  
|:---------:|:-------:|:-------:|  
| varchar2  | 2个bytes | 3个byte  |  
| nvarchar2 | 2个bytes | 1个bytes |  
|   char    | 2个byte  | 3个byte  |  
|   nchar   | 1个byte  | 1个byte  |

```SQL
# 查询数据库字符集编码
select userenv('language') from dual

```

### 数值类型
1. 可以存储整数、浮点数和实数，最高精度为38位。
2. 数值数据类型的声明语法：NUMBER [( p[, s])] （P表示精度，S表示小数点的位数）

### 日期类型
日期时间数据类型存储日期和时间值，包括年、月、日，小时、分钟、秒。
1. DATE - 存储日期和时间部分，精确到整个的秒
2. TIMESTAMP - 存储日期、时间和时区信息，秒值精确到小数点后6位


## 什么是oracle数据库？ {id="oracle_6"}
1. 数据库出现之前，我们可以通过文件化的方式把数据持久化到硬盘上。虽然我们可以通过文件数据持久化到硬盘上但对文件中的数据进行操作（增删改查 CRUD）的时候，会涉及到性能，安全性等问题。这时就缺少一个系统的管理软件，数据库软件就这样应运而生。
2. oracle是甲骨文公司开发的一款数据库软件（甲骨文公司收购了sun公司，jdk1.7之后由甲骨文公司维护）是对象关系型的数据库管理系统（ORDMS）。

## oracle数据库的主要特点 {id="oracle_7"}
1. 支持多用户，大事物量的数据处理
2. 数据的安全性和完整性的控制
3. 支持分布式数据处理
4. 可移植性
5. Oracle数据库基于客户端/服务器技术<发出请求的是客户端,响应的是服务器>

## oracle数据库结构 {id="oracle_8"}
oracle 数据库包括逻辑结构和物理结构。
1. 物理结构包含数据库中的一组操作系统文件。
2. 逻辑结构指数据库创建之后形成的逻辑概念之间的关系。

### oracle物理结构 {id="oracle_9"}
物理组件就是Oracle数据库所使用的操作系统物理文件，包括数据文件、控制文件、日志文件等。
1. 数据文件：数据文件用于存储数据库数据，如表、索引数据等。
2. 控制文件：控制文件是记录数据库物理结构的二进制文件。
3. 在线日志文件：记录对数据库的所有修改信息，用于故障恢复。

### oracle逻辑结构 {id="oracle_10"}
数据库的逻辑结构是从逻辑的角度分析数据库的组成，包括表空间、段、区、数据块等，用于组织和存储数据。
1. 表空间（Table Space）：Oracle中，最大的逻辑存储结构是表空间。表空间和物理上的数据文件相对应
2. 段（Segment）是一组盘区，这组盘区组成了被Oracle视为一个单位的数据库对象，比如表或索引。因此，段是数据库终端用户将处理的最小单位
3. 区（Area）：Oracle中，区是磁盘空间分配的最小单位
4. 块（Block）是用来管理存储空间的最基本单位，也是最小的逻辑存储单位
5. 模式对象（Model）：表、视图、序列、存储过程、函数等

## 什么是oracle伪列？ {id="oracle_14"}
Oracle 中伪列就像一个表列，但是它并没有存储在表中，伪列可以从表中查询但不能插入、更新和删除它们的值。

常用的伪列有ROWID和ROWNUM
1. ROWNUM是查询返回的结果集中行的序号，可以使用它来限制查询返回的行数
2. ROWID是表中行的存储地址，该地址可以唯一地标识数据库中的一行，可以使用ROWID 伪列快速地定位表中的一行

## 什么是oracle会话？ {id="oracle_11"}
Oracle的会话是数据库中的一个重要概念，它代表了客户端和数据库之间的交互过程。通过管理和维护会话，可以有效地管理数据库的并发访问和资源利用。
1. 会话是用户与Oracle 服务器的单个连接
2. 当用户与服务器建立连接时创建会话
3. 当用户与服务器断开连接时关闭会话

## oracle查询工具 {id="oracle_12"}
Oracle 提供的工具非常容易使用。Oracle 的查询工具包括：
1. SQL*Plus是Oracle最常用的工具之一，用于接受和执行SQL命令以及PL/SQL块，有DOS和WIN版
2. iSQL*Plus可以执行能用SQL*Plus完成的所有任务。该工具的优势在于能通过浏览器访问它（PORT：5560）
3. PL/SQL 是SQL 的扩展。PL/SQL 结合了SQL语言的数据操纵能力和过程语言的流程控制能力

## oracle聚合函数 {id="oracle_2"}
1. count() 统计数量
2. max() 最大值
3. min() 最小值
4. avg() 平均值
5. sum() 总和

## oracle数学函数 {id="oracle_3"}
1. ABS()  绝对值
2. CEIL() 上取整
3. FLOOR() 下取整
4. TRUNC() 截断函数
5. ROUND() 四舍五入函数
6. DBMS_RANDOM.VALUE ( [ min,max] )取随机值

## 字符串函数
1. CONCAT(str1,str2) || 连接两个字符串 
2. INITCAP(str) 返回字符串并将字符串的第一个字母变为大写
3. INSTR ( string1, string2 [, start_position [, nth_appearance ] ] )在一个字符串中搜索指定的字符,返回发现指定的字符的位置
4. LENGTH(str) 返回字符串的长度
5. LOWER(str) 返回字符串,并将所有的字符小写
6. UPPER(str) 返回字符串,并将所有的字符大写
7. RPAD|LPAD(str,length,char) 在字符串的右(左)边粘贴字符
8. RTRIM|LTRIM (str,search) 删除右(左)边出现的字符串
9. SUBSTR(str,start,count) 取子字符串,从start开始,取count个
10. REPLACE(string,s1,s2) 替换字符串
11. REVERSE( ) 反转字符串中的每个字符


## oracle转换函数 {id="oracle_4"}
1. to_char() 将number或者date类型转化为字符串
2. to_number() 将字符串转化为数值
3. to_date() 将字符串转化为日期类型

```SQL
select  to_char(birthday,'yyyy-mm-dd') from  student_info;
select  to_number('12.34') from dual;
select  to_date('2015-11-10 10:34:00','yyyy-mm-dd hh:mi:ss') from dual;

```

## oracle日期时间函数 {id="oracle_13"}
1. sysdate			用来得到系统的当前日期
2. systimestamp		用来得到系统的当前日期，精确到小数点后六位
3. add_months		或减去月份
4. last_day			日期所在月的最后一天
```SQL
select add_months(birthday,-2) from student_info;
select last_day(birthday) from student_info;

```


## char、varchar、nchar和nvarchar的区别 {id="datatype_diff"}
1. 定长或变长。有var前缀的，表示是实际存储空间是变长的。
    - char(n) 存储null或者n个字符，如果插入值非空而长度不足n则用空格补齐；若长度为10，则插入了3个字符，那么则会默认添加7个空格
    - varchar2(n) 存储0-n个字符，实际存储长度因字符集不同而变化
2. Unicode或非Unicode。前缀n（nation）就表示Unicode字符，多用于存储多国语言字符。Unicode字符集就是为了解决字符集这种不兼容的问题而产生的，它所有的字符都用两个字节表示。
3. 字符串类型的容量

| 数据类型           | 容量                  |
|----------------|---------------------|
| char，varchar   | 最多8000个英文，4000个汉字   |
| nchar，nvarchar | 可存储4000个字符，无论英文还是汉字 |

### 字符串类型使用
1. 如果数据量非常大，又能100%确定长度且保存只是ansi字符，那么char。
2. 能确定长度又不一定是ansi字符或者，那么用nchar。
3. 对于超大数据，如文章内容，使用nText。
4. 其他的通用nvarchar。

## oracle分页 {id="oracle_5"}
```sql

# 输出记录的序号
select rownum r,* from table_name;

输出前五条记录
select empno,* from table_name where rownum<=5

# 查询第2页
select * from (
    select t.*, rownum rn from (
        select * from table_name order by id desc
    ) t where rownum < 10) 
where rn >=5;

select * from (
    select t.*, rownum rn from (
        select * from table_name order by id desc
    ) t ) 
where rn >=5 and rownum < 10;

select * from (
    select t.*, rownum rn from (
        select * from table_name order by id desc
    ) t ) 
where between 5 and 10;

# rownum只支持以下查询。
# ROWNUM是一个序列，是oracle数据库数据库从数据文件或缓冲区中读取数据的顺序。一条记录则它取得第rownum值为1，第二条为2，依次类推。
# 如果你用>,>=,=,between...and这些条件，因为从缓冲区或数据文件中得到的第一条记录的rownum为1，则被删除，接着取下条，可是它的rownum还是1，又被删除，依次类推，便没有了数据。
select * from area where rownum =1

```

## oracle查询遍历树形结构
```sql
select * from extmenu start with pid=1 connect by prior id = pid;
```

## oracle连接不到服务器怎么办？ {id="oracle_connect"}

1. 无监听程序：开启服务
2. 用户名或者密码不对：检查用户名或者密码

### 连接不到服务请求：重启服务
1. 找到oracle安装目录 D:\software\oracle11g\product\11.2.0\dbhome_1\NETWORK\ADMIN
2. 将listener.ora以及tnsnames.ora移出文件夹，进行修改，将localhost改为计算机全名 -----重启服务试试
3. 修改监听 开始-Oracle - OraDb11g_home1---》配置和移植工具---》netManager--->将原来的监听删除，重新添加一个默认的就可以