# spring事务

1. 事务是恢复和并发控制的基本单位。
2. 事务是为了保证对同一数据表操作的一致性。
3. 事务有原子性、一致性、隔离性和持久性一共4个属性，这四个属性通常称为ACID特性。
4. mysql默认可重复读，oracle默认读已提交。
5. 在项目中使用spring的AOP来控制事务的，主要配置了事务的传播特性和service业务逻辑层的切面位置。

## 什么是事务?
在数据库中，所谓事务（Transaction）是指一组逻辑操作单元（unit）即一组sql语句。当这个单元中的一部分操作失败,整个事务回滚，只有全部正确才完成提交。

事务的开始和结束可以由用户显示控制。如果用户没有显示的定义事务，则有DBMS（数据库管理系统）按照缺省规定自动划分事务。和事务相关的语句有3个： BEGIN TRANSACTION，COMMIT，ROLLBACK。
1. 事务通常以BEGIN TRANSACTION开始，以COMMIT或ROLLBACK结束。
2. COMMIT表示提交，即提交事务的所有操作。具体的说就是将事务中所有对数据库的更新写回到磁盘上的物理数据库中去，事务正常结束。
3. ROLLBACK表示回滚，即在事务运行的过程中发生了某种故障，事务不能继续执行，系统将事务中对数据库的所有已完成的操作全部撤销，回滚到事务开始时的状态。这里的操作指对数据库的更新操作。

## 事务的四大特性
1. 原子性（Atomicity）：原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
2. 一致性（Consistency）：事务必须使数据库从一个一致性状态变换到另外一个一致性状态。(数据不被破坏)。一致性与原子性是密切相关的。
3. 隔离性（Isolation）：事务的隔离性是指一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
4. 持久性（Durability）：持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的。接下来的其他操作或故障不应该对其有任何影响。

## JDBC中的事务
事务默认是自动提交的。每次执行一个 SQL语句时，如果执行成功，就会向数据库自动提交，而不能回滚。     

为了让多个 SQL 语句作为一个事务执行，因此要配置jdbc事务。
1. 执行语句前调用 Connection 对象的 setAutoCommit(false)，以取消自动提交事务
2. 在所有的 SQL 语句都成功执行后，调用 commit 方法提交事务
3. 在出现异常时，调用 rollback 方法回滚事务。


## spring的事务隔离级别 {id="spring_2"}
我们都知道数据库隔离级别有4种，分别为读未提交、读已提交、可重复读、串行化。其实Spring也可以设置数据库隔离级别，Spring事务隔离级别比数据库事务隔离级别多一个default。

1. ISOLATION_DEFAULT（默认）：这是一个PlatformTransactionManager默认的隔离级别，使用数据库默认的事务隔离级别。另外四个与JDBC的隔离级别相对应。
2. READ_UNCOMMITTED（读未提交）：这是事务最低的隔离级别，它允许另外一个事务可以看到这个事务未提交的数据。这种隔离级别会产生脏读，不可重复读和幻读。
3. READ_COMMITTED（读已提交）：保证一个事务修改的数据提交后才能被另外一个事务读取，另外一个事务不能读取该事务未提交的数据，也是大多数数据库的默认值。这种事务隔离级别可以避免脏读出现，但是可能会出现不可重复读和幻读。
4. REPEATABLE_READ（可重复读）：在一个事务内两次读到的数据是不一样的，这种事务隔离级别可以防止脏读、不可重复读，但是可能出现幻读（mysql已经通过采用next-key锁解决了幻读问题）。它除了保证一个事务不能读取另一个事务未提交的数据外，还保证了不可重复读。
5. SERIALIZABLE（串行化）：这是花费最高代价但是最可靠的事务隔离级别，事务被处理为顺序执行。除了防止脏读、不可重复读外，还避免了幻读。并发性也最低，但最安全。


|       隔离级别       | 脏读 | 不可重复读 | 幻读 |
|:----------------:|:--:|:-----:|:--:|
| Read uncommitted | √  |   √   | √  |
|  Read committed  | ❌  |   √   | √  |
| Repeatable read  | ❌  |   ❌   | √  |
|   Serializable   | ❌  |   ❌   | ❌  |

## spring的事务传播行为 {id="spring_1"}
Propagation的key属性确定代理应该给哪个方法增加事务行为，这样的属性最重要的部份是传播行为。
1. PROPAGATION_REQUIRED：支持当前事务，如果当前有事务，就用当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择。也是默认的事务行为。当前指的是调用本方法的方法，也就是外层方法。 俗话讲：大哥给钱花，小弟有钱花；大哥不给钱花，如果小弟自己有钱，小弟花自己钱。
2. PROPAGATION_SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行。如果外层方法没有事务，就会以非事务进行执行。俗话讲：大哥给钱花，小弟有钱花。大哥不给钱花，小弟没钱花。
3. PROPAGATION_MANDATORY：支持当前事务，如果当前没有事务，就抛出异常。俗话讲：大哥不给钱花，小弟造反。
4. PROPAGATION_REQUIRES_NEW：如果当前存在事务，把当前事务挂起，自己新建事务。（就是把事务独立开来）俗话讲：大哥给钱花，小弟不要，小弟花自己的钱。
5. PROPAGATION_NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起，以非事务进行执行。俗话讲：大哥给钱花，小弟不要，不花钱。
6. PROPAGATION_NEVER：以非事务方式执行，如果当前存在事 务，则抛出异常。俗话讲：大哥给钱花，小弟造反。
7. PROPAGATION_NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。

## 描述下spring事务处理类
Spring框架支持事务管理的核心是事务管理器抽象，对于不同的数据访问框架（如Hibernate）通过实现策略接口PlatformTransactionManager，它由3个方法组成。
1. getTransaction()：返回一个已经激活的事务或创建一个新的事务（根据给定的TransactionDefinition类型参数定义的事务属性），返回的是TransactionStatus对象代表了当前事务的状态，该方法抛出TransactionException（未检查异常）表示事务由于某种原因失败。    
2. commit()：用于提交TransactionStatus参数代表的事务。
3. rollback()：用于回滚TransactionStatus参数代表的事务。


## spring提供几种事务实现？ {id="spring_3"}
一种编程式和三种声明式。编程式事务侵入性比较强，但处理粒度更细。
1. 一种编程式：基于底层 API txManager.getTransaction方式或基于TransactionTemplate。
2. 三种声明式：AOP（TransactionProxyFactoryBean）、基于AspectJ的声明式事务`<tx:advice>`和基于注解方式的声明式事务（@Transactional）    

```xml
<!-- 开启注解事务 -->
<tx:annotation-driven transaction-manager="transactionManager" />
```

## 如何使用springAOP进行事务控制？
通常把增删改以外的操作需要配置成只读事务，只读事务可以提高性能。
1. 在spring-common.xml里面通过 aop:config 里面先设定切面为service层
2. 之后引入 tx:advice（配置事务的方法、事物的传播特性和隔离级别等）
3. 在 tx:advice 引用 transactionManager（事务管理）
4. 在事务管理里再引入sessionFactory
5. sessionFactory注入 dataSource
6. 最后通过 aop:config 引入txAdvice。


```xml
<beans>
    <!-- 配置事务管理器   -->
    <bean id="transactionManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory">
            <ref bean="sessionFactory"/>
        </property>
    </bean>

    <!--  配置事务的传播特性  -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="add*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="insert*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="save*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="del*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="modify*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="update*" propagation="REQUIRED" rollback-for="Exception"/>
            <tx:method name="*" read-only="true"/>
        </tx:attributes>
    </tx:advice>

    <aop:config>
        <aop:pointcut id="allManagerMethod" expression="execution(* com.jk.service..*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="allManagerMethod"></aop:advisor>
    </aop:config>  
</beans>

```

##  JavaEE的事务类型有哪些？
一般J2EE服务器支持三种类型的事务管理。即：JDBC事务，JTA事务，容器管理事务。
1. JDBC事物接口：PlatformTransactionManager、AbstractPlatformTransactionManager和DataSourceTransactionManager。
2. JTA具有三个主要的接口：UserTransaction、JTATransactionManager和Transaction。
3. 容器级事务主要是由容器提供的事务管理，如：WebLogic/Websphere。

## JDBC和JTA事务区别
简单的说jta是多库的事务，jdbc是单库的事务。

### jdbc事务 {id="jdbc_1"}
JDBC事务由Connection对象控制管理。也就是说，事务管理实际上是在JDBC Connection中实现。事务周期限于Connection的生命周期。JDBC Connection 接口( java.sql.Connection )提供了两种事务模式：自动提交和手工提交。   
1. 自动提交：缺省是自动提交。一条对数据库的更新（增/删/改）代表一项事务操作，操作成功后，系统将自动调用commit()来提交，否则将调用rollback()来回滚。    
2. 手工提交：通过调用setAutoCommit(false)来禁止自动提交。这样就可把多个数据库操作的表达式作为一个事务，在操作完成后调 用commit()来进行整体提交，其中任何一个操作失败，都不会执行到commit()，并产生异常；此时可在异常捕获时调用rollback()进行回滚，以保持多次更新操作后，相关数据的一致性.   

JDBC 事务的一个缺点是事务的范围局限于一个数据库连接。一个JDBC事务不能跨越多个数据库。

### jta事务  
JTA(Java Transaction API)提供了跨数据库连接（或其他JTA资源）的事务管理能力。JTA事务管理则由JTA容器实现，J2ee框架中事务管理器与应用程序，资源管理器，以及应用服务器之间的事务通讯。    


## 什么是分布式事务？
简单的说，就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。

### 分布式事务的产生的原因
1. 数据库分库分表：当数据库单表一年产生的数据超过1000W，那么就要考虑分库分表，具体分库分表的原理在此不做解释，以后有空详细说，简单的说就是原来的一个数据库变成了多个数据库。这时候，如果一个操作既访问01库，又访问02库，而且要保证数据的一致性，那么就要用到分布式事务。
2. 应用SOA化：所谓的SOA化，就是业务的服务化。比如原来单机支撑了整个电商网站，现在对整个网站进行拆解，分离出了订单中心、用户中心、库存中心。对于订单中心，有专门的数据库存储订单信息，用户中心也有专门的数据库存储用户信息，库存中心也会有专门的数据库存储库存信息。这时候如果要同时对订单和库存进行操作，那么就会涉及到订单数据库和库存数据库，为了保证数据一致性，就需要用到分布式事务。


## 什么是脏数据、脏读、不可重复读和幻觉读？
1. 脏读: 指当一个事务正在访问数据，并且对数据进行了修改，而这种修改还没有提交到数据库中，这时另外一个事务也访问这个数据，然后使用了这个数据。因为这个数据是还没有提交的数据，那么另外一个事务读到的这个数据是脏数据，依据脏数据所做的操作可能是不正确的。
2. 不可重复读: 指在一个事务内，多次读同一数据。在这个事务还没有结束时，另外一个事务也访问该同一数据。那么在第一个事务中的两次读数据之间，由于第二个事务的修改，那么第一个事务两次读到的数据可能是不一样的。这样就发生了在一个事务内两次读到的数据是不一样的，因此称为是不可重复读。
3. 幻觉读: 指当事务不是独立执行时发生的一种现象，例如第一个事务对一个表中的数据进行了修改，这种修改涉及到表中的全部数据行。同时第二个事务也修改这个表中的数据，这种修改是向表中插入一行新数据。那么以后就会发生操作第一个事务的用户发现表中还有没有修改的数据行，就好象发生了幻觉一样。

---

1. 不可重复读（修改）：同样的条件，你读取过的数据，再次读取出来发现值不一样了 。
2. 幻读（新增或删除）：同样的条件，第 1 次和第 2 次读出来的记录数不一样。  