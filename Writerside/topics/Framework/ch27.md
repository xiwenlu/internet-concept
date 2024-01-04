# spring 配置


## 如何减少Spring XML的配置数量
使用Java配置文件，使xml中的bean转为Javaconfig文件方式   
注解的方式

## Spring 提供几种配置方式来设置元数据？ {id="spring_1"}
``将 Spring 配置到应用开发中有以下三种方式：``
1. 基于 XML 的配置
2. 基于注解的配置
3. 基于 Java 的配置


## 如何使用 XML 配置的方式配置 Spring？
在 Spring框架中，依赖和服务需要在专门的配置文件来实现，我常用的XML格式的配置文件。这些配置文件的格式通常用开头，然后一系列的bean定义和专门的应用配置选项组成。SpringXML配置的主要目的时候是使所有的 Spring 组件都可以用 xml 文件的形式来进行配置。    
这意味着不会出现其他的 Spring 配置类型（ 比如声明的方式或基于 Java Class 的配置方式），Spring 的 XML 配置方式是使用被 Spring 命名空间的所支持的一系列的 XML 标签来实现的。  
``Spring 有以下主要的命名空间：``context、 beans、jdbc、 tx、 aop、 mvc 和 aso。


```xml
class="org.springframework.web.servlet.view.BeanNameViewResolver"/>
class="org.springframework.web.servlet.view.json.MappingJackson2JsonView"/>
```

下面这个 web.xml 仅仅配置了DispatcherServlet，这件最简单的配置便能满足应用程序配置运行时组件的需求。

## 举例说明什么是Spring基于Java的配置？
Spring3.0之前都是基于XML配置的，Spring3.0开始可以几乎不使用XML而使用纯粹的java代码来配置Spring应用。

```java
@Configuration
@EnableTransactionManagement
public class AppConfig implements TransactionManagementConfigurer {
    @Bean
    public DruidDataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl("jdbc:mysql://localhost:3306/sqoop");
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUsername("root");
        dataSource.setPassword("abcd_123");
        return dataSource;
    }

    @Bean
    public JdbcTemplate jdbcTemplate(DruidDataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }

    @Bean
    public PlatformTransactionManager txManager() {
        return new DataSourceTransactionManager(dataSource());
    }

    @Override
    public PlatformTransactionManager annotationDrivenTransactionManager() {
        return txManager();
    }
}

```


## 如何用基于 Java 配置的方式配置 Spring？
Spring 对 Java 配置的支持是由@Configuration 注解和@Bean 注解来实现的。 由@Bean 注解的方法将会实例化、 配置和初始化一个新对象， 这个对象将由 Spring 的 IOC 容器来管理。    
@Bean 声明所起到的作用与元素类似。被@Configuration所注解的类则表示这个类的主要目的是作为 bean 定义的资源。 被@Configuration 声明的类可以通过在同一个类的内部调用    
@bean 方法来设置嵌入 bean 的依赖关系。    
``最简单的@Configuration 声明类请参考下面的代码：``

```java
@Configuration
public class AppConfig{
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

``对于上面的@Beans 配置文件相同的 XML 配置文件如下：``

``上述配置方式的实例化方式如下： ``     
利用 AnnotationConfigApplicationContext 类进行实例化
```java

public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```

``要使用组件组建扫描， 仅需用@Configuration 进行注解即可：``


```
@Configuration
@ComponentScan(basePackages = "com.gupaoedu")
public class AppConfig {
}
```

在上面的例子中，com.gupaoedu 包首先会被扫到， 然后再容器内查找被@Component 声明的类， 找到后将这些类按照 Sring bean 定义进行注册。     
如果你要在你的web应用开发中选用上述的配置的方式的话，需要用AnnotationConfigWebApplicationContext 类来读取配置文件，可以用来配置Spring的Servlet监听器ContrextLoaderListener 或者 Spring MVC 的 DispatcherServlet。

## 怎样用注解的方式配置 Spring？
Spring在2.5版本以后开始支持用注解的方式来配置依赖注入。可以用注解的方式来替代XML方式的 bean 描述，可以将bean描述转移到组件类的内部，只需要在相关类上、方法上或者字段声明上使用注解即可。注解注入将会被容器在XML注入之前被处理，所以后者会覆盖掉前者对于同一个属性的处理结果。注解装配在Spring中是默认关闭的。所以需要在Spring文件中配置一下才能使用基于注解的装配模式。如果你想要在你的应用程序中使用关于注解的方法的话， 请参考如下的配置。   
在标签配置完成以后，就可以用注解的方式在Spring中向属性、方法和构造方法中自动装配变量。      
``下面是几种比较重要的注解类型：``
1. ``@Required：`` 该注解应用于设值方法。
2. ``@Autowired：`` 该注解应用于有值设值方法、 非设值方法、 构造方法和变量。
3. ``@Qualifier：`` 该注解和@Autowired 注解搭配使用， 用于消除特定 bean 自动装配的歧义。
4. ``JSR-250 Annotations：`` Spring 支 持 基 于 JSR-250 注 解 的 以 下 注 解 ， @Resource、@PostConstruct 和 @PreDestroy。


## FileSystemResource和ClassPathResource有何区别？

在FileSystemResource中需要给出spring-config.xml文件在你项目中的相对路径或者绝对路径。在ClassPathResource中spring会在ClassPath中自动搜寻配置文件，所以要把ClassPathResource 文件放在ClassPath下。     
如果将spring-config.xml保存在了src文件夹下的话，只需给出配置文件的名称即可，因为src文件夹是默认。      
简而言之，ClassPathResource在环境变量中读取配置文件，FileSystemResource在配置文件中读取配置文件。




