# mybatis缓冲


## 简单的说一下MyBatis的一级缓存和二级缓存？ {id="mybatis_1"}
Mybatis首先去缓存中查询结果集，如果没有则查询数据库，如果有则从缓存取出返回结果集就不走数据库。Mybatis内部存储缓存使用一个HashMap，key为hashCode+sqlId+Sql语句。value为从查询出来映射生成的java对象      
Mybatis的二级缓存即查询缓存，它的作用域是一个mapper的namespace，即在同一个namespace中查询sql可以从缓存中获取数据。二级缓存是可以跨SqlSession的。

## 讲下MyBatis的缓存 {id="mybatis_2"}
MyBatis 提供了查询缓存来缓存数据，以提高查询的性能。MyBatis的缓存分为一级缓存和二级缓存。  
一级缓存是 SqlSession 级别的缓存  
二级缓存是 mapper 级别的缓存，多个 SqlSession 共享


``一级缓存``        
一级缓存是SqlSession级别的缓存，是基于HashMap的本地缓存。不同的SqlSession之间的缓存数据区域互不影响。    
一级缓存的作用域是SqlSession范围，当同一个SqlSession执行两次相同的 sql语句时，第一次执行完后会将数据库中查询的数据写到缓存，第二次查询时直接从缓存获取不用去数据库查询。当SqlSession执行insert、update、delete操做并提交到数据库时，会清空缓存，保证缓存中的信息是最新的。    
MyBatis默认开启一级缓存。

``二级缓存``    
二级缓存是mapper级别的缓存，同样是基于HashMap进行存储，多个SqlSession可以共用二级缓存，其作用域是mapper的同一个namespace。不同的SqlSession两次执行相同的namespace下的sql语句，会执行相同的sql，第二次查询只会查询第一次查询时读取数据库后写到缓存的数据，不会再去数据库查询。      
MyBatis 默认没有开启二级缓存，开启只需在配置文件中写入如下代码：

```
<settings> 
    <setting name="cacheEnabled" value="true"/>  
</settings>
```



## Mybatis的缓存介绍了解? {id="mybatis_3"}
正如大多数持久层框架一样，MyBatis 同样提供了一级缓存和二级缓存的支持。
1. 一级缓存: 基于PerpetualCache 的 HashMap本地缓存，其存储作用域为Session,当 Session flush 或 close之后，该Session中的所有 Cache 就将清空。
2. 二级缓存与一级缓存其机制相同，默认也是采用 PerpetualCache，HashMap存储，不同在于其存储作用域为Mapper(Namespace)，并且可自定义存储源，如 Ehcache。
3. 对于缓存数据更新机制，当某一个作用域(一级缓存Session/二级缓存Namespaces)的进行了 C/U/D 操作后，默认该作用域下所有 select 中的缓存将被clear。


## mybatis二级缓存
Mybatis的一级缓存是指SqlSession。一级缓存的作用域是一个SqlSession。Mybatis默认开启一级缓存。在同一个SqlSession中，执行相同的查询SQL，第一次会去查询数据库，并写到缓存中；第二次直接从缓存中取。当执行SQL时两次查询中间发生了增删改操作，则一级缓存清空。     
Mybatis的二级缓存是指mapper映射文件。二缓存的作用域是同一个namespace下的mapper映射文件内容，多个SqlSession共享。Mybatis需要手动设置启动二级缓存。在同一个namespace下的mapper文件中，执行相同的查询SQL，第一次会去查询数据库，并写到缓存中；第二次直接从缓存中取。当执行SQL时两次查询中间发生了增删改操作，则二级缓存清空。     mybaits的二级缓存是mapper范围级别，要在具体的mapper.xml中开启二级缓存。   
在核心配置文件 中加入:

```xml
<!-- 全局配置参数，需要时再设置 -->
<settings>
    <!-- 开启二级缓存 默认值为true -->
    <setting name="cacheEnabled" value="true"/>
</settings>
```

mapper.xml下的sql执行完成会存储到它的缓存区域（HashMap）。

```xml
<mapper namespace="cn.hpu.mybatis.mapper.UserMapper">
<!-- 开启本mapper namespace下的二级缓存 -->
<cache></cache>
```

## 开启二级缓存
SSM:        
1. 导入MyBatis-EhCache整合包     
2. classpath下添加EhCache配置文件（ehcache.xml）     
```xml
<ehcache>
    <diskStore path="F:\cache_test" />
    <defaultCache eternal="false" 
                  maxElementsInMemory="1000"
                  timeToIdleSeconds="20" 
                  timeToLiveSeconds="20" 
                  overflowToDisk="true"
                  maxEntriesLocalDisk="10000000"
                  diskExpiryThreadIntervalSeconds="20" 
                  memoryStoreEvictionPolicy="LRU" />
</ehcache>
```

3. MyBatis配置文件（SqlMapConfig.xml）打开二级缓存
```xml
<settings>
    <setting name="cacheEnabled" value="true"/>
</settings>
```

4. Mapper配置文件添加cache标签
```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

5. 缓存结果继承序列化接口
```java

public class User implements Serializable

```
springBoot:
1. 在pom.xml中引入cache依赖，添加如下内容：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

2. 在Spring Boot主类中增加@EnableCaching注解开启缓存功能


