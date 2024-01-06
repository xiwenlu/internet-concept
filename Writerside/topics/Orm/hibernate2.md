[//]: # (todo)

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
## Hibernate多表联查的实现方式核心代码
hibernate很多实现都是靠喜欢配关系，但是如果两张表，数据量都非常大的时候，并不合适配关系。

例如：student表和score表需要做联合查询。
1. sql: select s.id,s.name,sc.score from student as s,score as sc where s.id = sc.userId; （字段都是用的数据库中字段名称）
2. HQL: select s.id,s.name,sc.score from Student as s,Score as sc where s.id = sc.userId;(上面字段都是 javabean的属性)

如果我们按1）查询的话，必须调用 session.createSQLQuery();方法    
如果按2）查询，还是调用 session.createQuery();    
只是要注意，平时我们查询的时候，例如：“from Student ”查询的结果集 封装的全都是student对象，但是2）执行的结果集里面不是对象，而是一系列数组。需要转换成需要的样式。
