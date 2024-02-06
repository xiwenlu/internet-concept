# spring模块


## 谈谈spring框架几个主要部分组成 {id="spring_3"}
Spring core/beans/context/aop/jdbc/tx/web mvc/orm


## 描述下springJDBC的架构
Spring JDBC提供了一套JDBC抽象框架，用于简化JDBC开发。       
Spring主要提供JDBC模板方式、关系数据库对象化方式、SimpleJdbc方式、事务管理来简化JDBC编程。

<img src="SpringJDBC.png" alt=""/>

``support包：``提供将JDBC异常转换为DAO非检查异常转换类、一些工具类如JdbcUtils等。   
``datasource包：``提供简化访问JDBC   数据源（javax.sql.DataSource实现）工具类，并提供了一些DataSource简单实现类从而能使从这些DataSource获取的连接能自动得到Spring管理事务支持。     
``core包：``提供JDBC模板类实现及可变部分的回调接口，还提供SimpleJdbcInsert等简单辅助类。    
``object包：``提供关系数据库的对象表示形式，如MappingSqlQuery、SqlUpdate、SqlCall


## jdbcTemplate有哪些主要方法 {id="jdbctemplate_1"}
CRUD操作全部包含在JdbcTemplate  
ResultSetExtractor/RowMapper/RowCallbackHandler


## jdbcTemplate支持哪些回调类
1. RowMapper是一个精简版的ResultSetExtractor，RowMapper能够直接处理一条结果集内容，而ResultSetExtractor需要我们自己去ResultSet中去取结果集的内容，但是ResultSetExtractor拥有更多的控制权，在使用上可以更灵活；
2. 与RowCallbackHandler相比，ResultSetExtractor是无状态的，他不能够用来处理有状态的资源。
