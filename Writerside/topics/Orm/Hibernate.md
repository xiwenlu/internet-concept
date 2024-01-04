# hibernate

## hibernate(冬眠)的概述 {id="hibernate_1"}
1. hibernate是一个对JDBC进行轻量级封装的持久层框架
2. hibernate是全自动的(将常用方法进行了封装，不用手动进行结果集的解析)
3. hibernate是跨数据库的，
4. hibernate的开发效率高于jdbc，
5. hibernate使用的hql语句，
6. 但是在最后操作数据库是hql语句会转化成sql语句去执行，
7. hibernate操作的是对象from实体类名


## Hibernate的作用 {id="hibernate_2"}
它是一个ORM的实现，是面向对象编程与关系型数据库之间的桥梁,是以对象的形式操作数据库。Hibernate是跨数据库的。

## hql概述
HQL是Hibernate Query Language（Hibernate 查询语言）的缩写，提供更加丰富灵活、更为强大的查询能力；HQL更接近SQL语句查询语法。

--- 

hibernate所使用的语句是hql语句，它是将操作的表改为映射的实体类来操作，hql语句最终会被解析为sql语句。

## hibernate两大核心配置文件 {id="hibernate_3"}
1. 核心控制文件 `hibernate.cfg.xml`
2. 实体类映射文件 `*.hbm.xml`

## hibernate的三种状态 {id="hibernate_18"}
### transient\['trænzɪənt](临时状态)    
在创建对象即在new之后，在save之前的状态，session中和数据库中都不存在该数据。
   
瞬时对象特点
1. 不和 Session 实例关联
2. 在数据库中没有和瞬时对象关联的记录       

瞬时对象转为持久对象
1. 通过 Session 的 save() 和 saveOrUpdate() 方法把一个瞬时对象与数据库相关联，这个瞬时对象就成为持久化对象。
2. 使用 find(),get(),load() 和 iterater() 等方法查询到的数据对象，将成为持久化对象。

### persistent\[pə'sɪst(ə)nt](持久化状态)
在save之后，session关闭之前，session中和数据库中都存在对应的数据。
   
持久化对象的特点    
1. 和 Session 实例关联
2. 在数据库中有和持久对象关联的记录

### detached\[dɪ'tætʃt](托管状态/游离状态)
session关闭之后，session不存在，数据中有对应的数据。

托管对象特点
1. 本质上和瞬时对象相同
2. 只是比瞬时对象多了一个数据库记录标识值 id。  

持久对象转为托管对象： 当执行 close() 或 clear(),evict() 之后，持久对象会变为托管对象。   
瞬时对象转为持久对象： 通过 Session 的 update(),saveOrUpdate() 和 lock() 等方法，把托管对象变为持久对象。

## hibernate中表与表之间关联关系 {id="hibernate_4"}
1. 一对一 one-to-one
2. 一对多 one-to-many
3. 多对一 many-to-one
4. 多对多 many-to-many

## hibernate常用注解 {id="hibernate_5"}
1. @Entity 声明实体bean，每一个持久化POJO类都是一个实体bean,这可以通过在类的定义中使用@Entity注解来进行声明:    
2. @Id 注解则声明了该实体bean的标识属性，对应相应表使用id列作为主键列     
3. @Table 是类一级的注解,通过@Table注解可以为实体bean映射指定表(table),目录(catalog)和schema的名字. 如果没有定义@Table,那么系统自动使用默认值：实体的短类名(不附带包名).        
4. @Transient 自动生成表时忽略某个字段    
5. @GeneratedValue 定义该标识符的生成策略

| 策略       | 描述                                                     |
|----------|--------------------------------------------------------|
| AUTO     | 可以是identity column类型,或者sequence类型或者table类型,取决于不同的底层数据库 |
| TABLE    | 使用表保存id值                                               |
| IDENTITY | identity column                                        |
| SEQUENCE | sequence                                               |

6. @OneToOne 注解可以建立实体bean之间的一对一的关联.    
7. @ManyToOne 注解来定义多对一关联

## hibernate主键生成策略并分别描述 {id="hibernate_6"}
1. Increment：先查询出最大id，在此基础上加1；hibernate框架会自动处理
2. Sequence(Oracle) ：oracle 数据库会自动处理
3. Assigned：人工指派（重复插入数据时会违反数据唯一性）
4. native(数据库本地生成策略，适用多个数据库)适用于mysql.oracle,sqlserver, 如果是orade数据库则默认使用的序列名为hibernate-sequence
5. uuid:生成一个32位，不会重复的主键，可以达到真正的跨数据库（通常来说对应的应该是string数据类型）
6. foreign：通常在一对一主键的时候使用，基于外键的主键生成策略


## hibernate缓存 {id="hibernate_7"}
一级缓存，即session缓存也叫事务级别的缓存以及二级缓存sessionFactory，即应用级别的缓存,还有查询缓存即三级缓存。    
1. 一级缓存的生命周期和session的生命周期保持一致，hibernate默认就启用了一级缓存，不能将其关闭，可以通过session.clear()和session.evict(object)来管理一级缓存。其中get,load,iterate都会使用一级缓存，一级缓存缓存的是对象。    
2. 二级缓存的生命周期和sessionFactory的生命周期保持一致，可以跨session,被多个session共享，hibernate3默认开启二级缓存,也可以手动开启并指定缓存插件如ehcache,oscache等。二级缓存也只能缓存对象。        
3. 三级缓存也叫查询缓存，查询缓存是针对普通属性结果集的缓存,对实体对象的结果集只缓存id。对query.list()起作用，query.iterate不起作用，也就是query.iterate不使用查询缓存。

--- 

1. 一级缓存就是Session级别的缓存，一个Session做了一个查询操作，它会把这个操作的结果放在一级缓存中，如果短时间内这个session（一定要同一个session）又做了同一个操作，那么hibernate直接从一级缓存中拿，而不会再去连数据库，取数据；
2. 二级缓存就是SessionFactory级别的缓存，顾名思义，就是查询的时候会把查询结果缓存到二级缓存中，如果同一个sessionFactory创建的某个session执行了相同的操作，hibernate就会从二级缓存中拿结果，而不会再去连接数据库；
3. Hibernate中提供了两级Cache，第一级别的缓存是Session级别的缓存，它是属于事务范围的缓存。这一级别的缓存由hibernate管理的，一般情况下无需进行干预；第二级别的缓存是SessionFactory级别的缓存，它是属于进程范围或群集范围的缓存。这一级别的缓存可以进行配置和更改，并且可以动态加载和卸载。 Hibernate还为查询结果提供了一个查询缓存，它依赖于第二级缓存；

## Ehcache的介绍
1. Ehcache是一个非常轻量级的缓存实现，而且从1.2之后就支持了集群，而且是hibernate默认的缓存provider。EhCache 是一个纯Java的进程内缓存框架，具有快速、精干等特点，是Hibernate中默认的CacheProvider。
2. Ehcache的分布式缓存有传统的RMI，1.5版的JGroups，1.6版的JMS。分布式缓存主要解决集群环境中不同的服务器间的数据的同步问题。
3. 使用Spring的AOP进行整合，可以灵活的对方法的返回结果对象进行缓存。
4. CachingFilter功能可以对HTTP响应的内容进行缓存。
5. 主要特性
   1. 快速
   2. 简单
   3. 多种缓存策略
   4. 缓存数据有两级：内存和磁盘，因此无需担心容量问题
   5. 缓存数据会在虚拟机重启的过程中写入磁盘
   6. 可以通过RMI、可插入API等方式进行分布式缓存
   7. 具有缓存和缓存管理器的侦听接口
   8. 支持多缓存管理器实例，以及一个实例的多个缓存区域
   9. 提供Hibernate的缓存实现等等。


## hibernate的运行原理 {id="hibernate_8"}
首先通过configuration去加载hibernate.cfg.xml这个配置文件，根据配置文件的信息去创建sessionFactory，sessionFactory是线程安全的，是一个session工厂，用来创建session,session是线程不安全的，相当于jdbc的connection，最后通过session去进行数据库的各种操作，在进行操作的时候通过transaction进行事务的控制。

--- 
1. 通过configuration加载了hibernate.cfg.xml文件，
2. 加载文件后得到sessionFactory，（sessionFactory是线程安全的）
3. 然后通过sessionFactory得到session（session是线程不安全的，相当于jdbc中connection）
4. 通过session操作对象，来操作数据库，最后通过Transaction来进行事务的控制

---

1. 通过configuration加载了hibernate.cfg.xml文件，加载文件后得到sessionFactory，（sessionFactory是线程安全的）
2. 然后通过sessionFactory得到session（session是线程不安全的，相当于jdbc中connection）
3. 通过session操作对象，来操作数据库，最后通过Transaction来进行事务的控制


## hibernate的核心 {id="hibernate_9"}
<img src="hibernate-core.png" alt=""/>

从上图中，我们可以看出hibernate六大核心接口，两个主要配置文件，以及他们直接的关系。hibernate的所有内容都在这了。那我们从上到下简单的认识一下，每个接口进行一句话总结。
1. configuration接口:负责配置并启动hibernate
2. sessionFactory接口:负责初始化hibernate
3. session接口:负责持久化对象的crud操作
4. transaction接口:负责事务
5. query接口和criteria接口:负责执行各种数据库查询

注意：Configuration实例是一个启动期间的对象，一旦SessionFactory创建完成它就被丢弃了。


## hibernate中的五大核心类和接口 {id="hibernate_10"}
1. Configuration(类)     
   加载配置文件hibernate.cfg.xml文件中的配置信息，从而得到：
   1. hibernate的底层信息：数据库连接，jdbc驱动，方言（dialect），用户名 ，密码
   2. hibernate的映射文件（*.hbm.xml）

2. SessionFactory(接口)      
   通过configuration创建的sessionFactory，可以用来获得session openSession(); sessionFactory是线程安全的，里面保存了数据的配置信息和映射关系
3. Session(接口)    
   不是线程安全的，相当于jdbc中connection，我们可以使用session来操作数据库负责保存、更新、删除、加载和查询对象，是一个非线程安全的，避免多个线程共享一个session，是轻量级，一级缓存。
4. Transaction(接口)：

```java
session.beginTransaction(); // 由于hibernate增删改需要使用事务所以这里要开启事务
session.getTransaction().commit(); // 提交
```
我们一般使用Transaction来进行事务的管理commit（提交）rollback（回滚）

5. Query(接口)：我们一般用来进行数据的查询操作


##  get和load的区别，以及注意事项
1. load是延迟加载（懒加载，按需加载）调用load方法后不会立即发送SQ语句，当访问实体属性的时候才会去发送SQL语句，如果访问的实体不存在，则返回ObjectNotFoundException（对象不存在异常）
2. get 是立即加载，当调用get方法后立即发送sql语句，当访问的实体不存在的时候，则返回null,get(Class entityClass,Serializable id)根据主键加载特定持久化实例。

在程序中一般先用 Assert.isTrue()断言id是否大于0，若大于0继续执行，若查到数据，则返回实例，否则返回空不同于load，load若有数据则返回实例，否则报出ObjectNotFoundException异常，相比来说get效率高些。     
注意：load支持懒加载通过lazy="true"设置，如果lazy="false"，则load不再进行懒加载，默认情况下lazy="true"。


## hibernate中query对象的常用方法
1. query对象.executeUpdate() 方法对数据进行修改或者删除    
2. query对象.uniqueResult() 方法返回单条记录，获取唯一结果集    
3. query对象.setFirstResult() 方法设置结果集开始位置，查询开始条数
4. query对象.setMaxResults() 方法设置每页查询条数  
5. query对象.list() 方法返回查询结果集


## hibernate中session对象的常用方法
1. find(String queryString) 查询返回list结果集 ,根据HQL查询字符串来返回实例集合find方法在执行时会先查找缓存,如果缓存找不到再查找数据库,如果再找不到就会返回null。
2. save(Object entity) 添加保存新的实例
3. saveOrUpdate() 添加或修改 有id是修改 没id 是更新
4. delete(Object entity) 删除 删除指定的持久化实例
5. update(Object entity) 修改 更新实例的状态 实例必须为持久化状态
6. get(Class entityClass,Serializable id) 根据ID查询 根据主键加载特定持久化实例 
7. load() 根据唯一标识获得对象延迟加载

## Hibernate的乐观锁、悲观锁理解 {id="hibernate_11"}
1. 悲观锁(Pessimistic Lock)      
每次去查询数据的时候都认为别人会修改，所以每次在查询数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁。传统的关系型数据库里边就用到了这种锁机制，比如通过select....for update进行数据锁定。       
2. 乐观锁(Optimistic Lock)     
每次去查询数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号，时间戳等机制。乐观锁适用于读比较多的应用类型，这样可以提高吞吐量。

两种锁各有优缺点，不可认为一种好于另一种，像乐观锁适用于写比较少的情况下，即冲突真的很少发生的时候，这样可以省去了锁的开销，加大了系统的整个吞吐量。但如果经常产生冲突，上层应用会不断的进行retry，这样反倒是降低了性能，所以这种情况下用悲观锁就比较合适。


## Hibernate悲观锁 {id="hibernate_12"}
Hibernate 通过使用数据库的 for update 子句实现了悲观锁机制。 Hibernate 的加锁模式有：
1. LockMode.NONE ：无锁机制。
2. LockMode.WRITE ：Hibernate 在 Insert 和 Update 记录的时候会自动获取。
3. LockMode.READ ：Hibernate 在读取记录的时候会自动获取。

## Hibernate乐观锁 {id="hibernate_13"}
Hibernate为乐观锁提供了3中实现
1. 基于version
2. 基于timestamp
3. 为遗留项目添加添加乐观锁

## hibernate的优/缺点 {id="hibernate_14"}
优点：    
1. 更加对象化        
   以对象化的思维操作数据库，我们只需要操作对象就可以了，开发更加对象化。
2. 移植性         
   因为hibernate做了持久层的封装，你就不知道数据库，你写的所有的代码都具有可复用性。
3. hibernate是一个没有侵入性的框架，没有侵入性的框架我们称为轻量级框架。         
   对比Struts的Action和ActionForm，都需要继承，离不开Struts。hibernate不需要继承任何类，不需要实现任何接口。这样的对象叫POJO对象。
4. hibernate代码测试方便。
5. 提高效率，提高生产力。

缺点：         
1. 使用数据库特性的语句，将很难调优
2. 对大批量数据更新存在问题
3. 系统中存在大量的攻击查询功能

## Hibernate中两种事务类型 {id="hibernate_15"}
jdbc事务、JTA事务

## hibernate的两种事务管理jdbc和jta方式
hibernate的两种事务管理jdbc 和jta方式。下边说说两者的区别
1. 说明一下jdbc和jta方式事务管理的区别      
JDBC事务由Connection管理，也就是说，事务管理实际上是在JDBC Connection 中实现。事务周期限于Connection的生命周期之内。      
JTA 事务管理则由 JTA 容器实现，JTA 容器对当前加入事务的众多Connection 进行调度，实现其事务性要求。JTA的事务周期可横跨多个JDBC Connection生命周期。

2. 在了解jdbc和jta事务的基础上，再来讨论hibernate的两种事务      
对于基于JDBC Transaction的Hibernate 事务管理机制而言，事务管理在Session 所依托的JDBC Connection 中实现，事务周期限于Session的生命周期。         
对于基于JTA事务的Hibernate而言，JTA事务横跨可横跨多个Session。      
3. hibernate中写法的不同           
jdbc的写法
```java
public void saveUser()   {
   Session session = sessionFactory.openSession();
   Transaction tx = session.beginTransaction();
   session.save(user);
   tx.commit();
   session.close();
} 
```
必须在session.close()之前commit或者rollback

jta写法    
```java
public void saveUser()   {
   Session session = sessionFactory.openSession();
   Transaction tx = session.beginTransaction();
   session.save(user);
   session.close(); 
   Session session1 = sessionFactory.openSession();
   session1.save(user1);
   session.close();
   tx.commit();
} 
```
commit和rollback可以在session.close()之后执行. 同时应该注意的一点是，事务是不能嵌套的，在使用jta的事务的情况下，如果要让一个事务跨越两个session,则必须在两个session的外层开始事务和完成事务。而不能再在session内部开始事务和完成事务。


## hibernate配置文件设置方言的关键字(dialect)
```xml
<!-- 数据库方言***记过-->
<!-- oracle 方言org.hibernate.dialect.Oracle9Dialect -->
<!-- mysql 方言org.hibernate.dialect.MySQLDialect -->
<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
```

## Hibernate中两种占位符是(?)和(:变量名) {id="hibernate_16"}
1. ?的赋值方式：  query对象.setParameter(index,value)
2. :变量名赋值方式：  query对象.setParameter("变量名",value)



## SSH运行原理 {id="ssh_1"}
1. 启动tomcat时加载web.xml文件，核心控制器FilterDispatcher会加载并且解析struts.xml文件
2. 从页面发送一个请求，核心控制器FilterDispatcher会根据后缀名进行拦截
3. FilterDispatcher会根据配置信息把请求发送给具体的Action类中的指定方法
4. 执行相关的业务逻辑，并且返回一个String字符串
5. 配置文件会根据result中name的属性值与返回的字符串进行匹配，匹配成功则跳转到相对应的jsp页面


## hibernate 的搭建步骤 {id="hibernate_17"}
1. 导入支持框架的jar包
2. 配置xml配置文件
   1. hibernate.cfg.xml 核心控制文件
   2. javaBean.hbm.xml 实体类映射文件（记得将映射文件引入核心控制器文件）
   3. javabean.hbm.xml 一般与实体类放一起
3. 创建实体类

## SSH框架的搭建步骤
1. 创建web项目
2. 导入ssh相应的jar包
3. 创建包结构，面向接口
4. 导入配置
   + 在web.xml中添加Struts2的核心控制器和spring的监听
   + 将struts.xml、hibernate.cfg.xml、applicationContext.xml配置文件放在src下 
   + 因为applicationContext.xml会加载解析hibernate.cfg.xml所以需要将hibernate.cfg.xml文件配置完整
   + 将javabean.hbm.xml映射文件及相应的实体类配置好
4. action类需要继承ActionSupport，action类中的方法必须是public修饰，返回值类型必须是string ，没有参数
5. service接口注入到spring中由spring管理，此时action中需要私有化此service属性并生成get、set方法。
6. dao 接口注入到spring中由spring管理，此时service需要私有化此dao属性并生成get、set方法，dao层需继承HibernateDaoSupport 

--- 

1. 首先在web.xml中通过contextLoaderListener来融入spring并加载spring相关配置文件
2. 同样配置struts2前端总控制器filterDispatcher来过滤相关的请求并加载struts.xml
3. action继承ActionSupport 然后引入struts-spring-plugin.jar包并在配置文件中注入service 提供get set 方法
4. 通过spring配置sessionFactory 读取hibernate.cfg.xml 数据库信息,这时hibernate.cfg.xml必须配置完全
5. dao层继承HibernateDaoSupport 在spring配置文件中dao注入sessionFactory
6. 在applicationContext.xml中通过byName自动注入service和dao

---
// todo

1. 首先导入SSH框架整合所需的jar包: Spring的jar包,Hibernate 的jar包,第三方jar包（日志包）,数据库的jar包

<img src="hibernate-package.png" alt=""/>
   
2. 配置web.xml,手动创建一个config文件夹用与存放配置的文件，这里方便说明记为cf,配置Spring的IOC的容器:
```xml
<context-param>
   <param-name>contextConfigLocation</param-name>
   <param-value>classpath:xx.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
其中xx.xml文件是需要你在cf中手动创建的配置文件，里面具体内容后面配置，这里相当于是告诉系统文件在哪，这里为了说明方便命名为spring.xml
1. 配置SpringMVC的控制器-DispatcherServlet
```xml
<servlet>
   <servlet-name>dispatcherServlet</servlet-name>
   <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
   <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:xxx.xml</param-value>
   </init-param>
   <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
   <servlet-name>dispatcherServlet</servlet-name>
   <url-pattern>/*</url-pattern>
</servlet-mapping>
```

4. 这里的xxx.xml同上，可自己命名，这里命名为springMVC.xml配置编码方式过滤器     
其中filter-class里的名字不用记，可以通过ctrl+shift+T输入CharacterEncodingFilter获得，且这一步需放在所有过滤器最前面，才有效果
```xml
<filter>
   <filter-name>characterEncodingFilter</filter-name>
   <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
   <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
   </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```   

5. 配置 HiddenHttpMethodFilter过滤器     
其中filter-name里的类名可以通过ctrl+shift+T输入HiddenHttpMethodFilter获得，由于浏览器form表单只支持GET与POST请求，而DELETE、PUT等method并不支持，Spring3.0添加了一个过滤器，可以将这些请求转换为标准的http方法，使得支持GET、POST、PUT与DELETE请求，该过滤器为HiddenHttpMethodFilter
```xml
<filter>
   <filter-name>hiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
6. 配置SpringMVC。导入命名空间打开我们自己定义的配置文件springMVC.xml选择Namespace，勾选context和MVC 添加组成扫描,包含过滤注解
```xml
<context:component-scan base-package="com.jikexueyuancrm" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:include-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice" />
</context:component-scan>
```

添加视图解析器，其中class类名可以通过ctrl+shift+T输入InternalResourceViewResolver获得
```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/"></property>
    <property name="suffix" value=".jsp"></property>
</bean>
```
配置静态资源
```xml
<mvc:default-servlet-handler />
```

7. 配置注解：
```xml
<mvc:annotation-driven />
```
8. 配置Spring，导入命名空间。打开我们自己定义的配置文件spring.xml选择Namespace，勾选context(上下文)和tx(事物) 添加组成扫描,排除被SpringMVC包含的过滤注解
```xml
<context:component-scan base-package="com.jikexueyuancrm" use-default-filters="false">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:exclude-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice" />
</context:component-scan>
```

9. 配置数据库
```xml
<context:property-placeholder location="classpath:db.properties" />
```
数据库内容
```yaml
jdbc.user=root
jdbc.passowrd=
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.jdbcUrl=jdbc:mysql:///jianlam
```

10. 配置DataSource-数据库关联的东西，这里用到cp30的包，C3P0是一个开源的JDBC连接池，它实现了数据源和JNDI绑定，支持JDBC3规范和JDBC2的标准扩展。
```xml
<bean class="com.mchange.v2.c3p0.ComboPooledDataSource" id="dataSource">
    <property name="user" value="${jdbc.user}"></property>
    <property name="password" value="${jdbc.passowrd}"></property>
    <property name="driverClass" value="${jdbc.driverClass}"></property>
    <property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
</bean>

```
    
11. 配置hibernate整合    
需要orm包-Object Relational Mapping 对象关系映射，POJO（Plain Ordinary Java Object）简单的Java对象，实际就是普通JavaBeans
```xml
<beanclass="org.springframework.orm.hibernate4.LocalSessionFactoryBean"id="sessionFactory">
<!-- 配置数据源 -->
<property name="dataSource" ref="dataSource"></property>
<!-- 找到实体包(pojo) -->
<property name="namingStrategy">
    <bean class="org.hibernate.cfg.ImprovedNamingStrategy"></bean>
</property>
<property name="packagesToScan" value="com.jianlam.entity"></property>
<!-- 配置hibernate常用属性 -->
<property name="hibernateProperties">
    <props>
        <!-- 数据库的方言 -->
        <prop key="hibernate.dialect">org.hibernate.dialect.MySQLInnoDBDialect</prop>
        <prop key="hibernate.show_sql">true</prop>
        <prop key="hibernate.format_sql">true</prop>
        <prop key="hibernate.hbm2ddl.auto">update</prop>
    </props>
</property>
</bean>
   <beanclass="org.springframework.orm.hibernate4.LocalSessionFactoryBean"id="sessionFactory">
   <!-- 配置数据源 -->
   <property name="dataSource" ref="dataSource"></property>
   <!-- 找到实体包(pojo) -->
   <property name="namingStrategy">
        <bean class="org.hibernate.cfg.ImprovedNamingStrategy"></bean>
   </property>
   <property name="packagesToScan" value="com.jianlam.entity"></property>
   <!-- 配置hibernate常用属性 -->
   <property name="hibernateProperties">
      <props>
         <!-- 数据库的方言 -->
         <prop key="hibernate.dialect">org.hibernate.dialect.MySQLInnoDBDialect</prop>
         <prop key="hibernate.show_sql">true</prop>
         <prop key="hibernate.format_sql">true</prop>
         <prop key="hibernate.hbm2ddl.auto">update</prop>
      </props>
   </property>
</bean>
```

12. 配置hibernate事物管理器
```xml
<bean id="transactionManager" class="org.springframework.orm.hibernate4.HibernateTransactionManager">
    <property name="sessionFactory" ref="sessionFactory"></property>
</bean>
```

13. 创建好实体类和数据库表，查看连接操作数据库是否能正常运作，若没有问题则配置成功！
```java 
private spring ctx = null;

@Test
public void testDataSource() throws SQLException {
   //检查spring配置
   ctx = new ClassPathXmlApplicationContext("spring.xml");
   //  System.out.println(ctx);
   //检查数据库连接
   DataSource dataSource = ctx.getBean(DataSource.class);
   // System.out.println(dataSource.getConnection().toString());
   //检查hibernate配置
   SessionFactory sessionFactory = ctx.getBean(SessionFactory.class);
   System.out.println(sessionFactory);
   Session session = sessionFactory.openSession();
   Transaction tx = session.beginTransaction();
   //测试数据库
   Users user = new Users("jianlam", "123456");
   session.save(user);
   tx.commit();
   session.close();
}
```

## 文件上传注意事项
1. 表单中必须添加属性enctype="multipart/form-data" 
2. action 中接收文件的属性有两个
   + File 类型的属性接收文件实体
   + String类型的文件名属性接收文件名信息
   + 注意：接收文件名的属性名称是接收File实体的属性名称+FileName ;
3. 数据库中存储的是上传后的路径
4. 前台回显时是虚拟路径+数据库存储的文件路径
5. 如果修改时不修改图片，只需要在action中判断file属性是否为空

## jdbc和hibernate的区别
hibernate是jdbc轻量级的封装,hibernate基于jdbc        
jdbc
1. jdbc是纯手工的(sql语句，对于结果集的解析)
2. 执行效率高于hibernate，
3. 使用的sql语句，
4. jdbc是直接操作数据库中的表  select * from 表名

hibernate
1. hibernate是一个对JDBC进行轻量级封装的持久层框架
2. hibernate是全自动的(将常用方法进行了封装，不用手动进行结果集的解析)
3. hibernate是跨数据库的，
4. hibernate的开发效率高于jdbc，
5. hibernate使用的hql语句，
6. 但是在最后操作数据库是hql语句会转化成sql语句去执行，
7. hibernate操作的是对象from实体类名

## Hibernate 多表联查的实现方式核心代码
hibernate很多实现都是靠喜欢配关系，但是如果两张表，数据量都非常大的时候，并不合适配关系。

例如：student表和score表需要做联合查询。
1. sql: select s.id,s.name,sc.score from student as s,score as sc where s.id = sc.userId; （字段都是用的数据库中字段名称）
2. HQL: select s.id,s.name,sc.score from Student as s,Score as sc where s.id = sc.userId;(上面字段都是 javabean的属性)

如果我们按1）查询的话，必须调用 session.createSQLQuery();方法    
如果按2）查询，还是调用 session.createQuery();    
只是要注意，平时我们查询的时候，例如：“from Student ”查询的结果集 封装的全都是student对象，但是2）执行的结果集里面不是对象，而是一系列数组。需要转换成需要的样式。
