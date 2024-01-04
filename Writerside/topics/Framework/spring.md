# Spring

spring中通过scope 属性的singleton设置单例，prototype设置多例

## Spring源码看过吗
等待补充

## 谈谈你对spring框架的理解aop、ioc事物怎配置的
`spring`据我了解在所有框架中都有 spring 的存在，它可以把很多框架很好的整合起来，我们用的最多的就是 ioc 和 aop 这两大模块，ioc 主要就是依赖注入这一块，包括属性注入和构造函数注入等，其实 ioc 最大的好处，是每次创建 bean 对象的时候，默认都是单例模式，这样减少了 java 频繁创建对象，占用大量内存。通过设置 scope 属性也可以设置为原型模式（也就是多利模式）。aop  这一块用的也挺多的，它主要包括了安全控制，日志记录，性能统计和事务处理等  

`spring` ：主要有两个核心 aop 和 ioc，分别为控制反转/依赖注入和面向切面编程思想，工作过程中主要用到 ioc 注入这块比较多，通过 spring 的注入能够更加方便维护bean之间的关系，大多数的 bean 实例都是单例的，很好的节省的对内存的消耗。spring 的注入方式主要用到过构造函数注入，属性注入，注入数据的类型比较多，常用对象注入，也使用到过 list 集合注入，使用比较灵活。  

ioc 是控制反转，依赖注入，主要用来维护 bean 之间的注入关系，使用工厂模式创建 bean 的实例，然后再根据 xml 中配置的 bean 的注入关系为创建好的实例注入 bean 对象。  

spring 的 aop 面向切面编程是基于代理模式实现的，通过我的工作经验来看，它其实就是把一部分公用的代码段提取到一个实现类中，让程序在运行过程中把代码再拼接成完整代码执行，增强代码的可读性和高复用性，尽可能的避免重复代码的出现，但是并不是所有的业务都适合使用 spring 的 aop 功能。  

事物控制这方面：在 spring-common.xml 在 `<aop:config>` 里面设定切面为service层引入配置方法的标签 `<tx:advice>` (这个标签里面配置了使用事物的方法，事物的传播特性，隔离级别等)，`<tx:advice>` 里面又引入了 transactionManager，transaction 又引入 sessionFactory，sessionFactory 注入 dataSource(数据源/数据库连接池)。  

(spring 原理 这块我也有一定的了解 ，使用 spring 这么长时间，我认为 spring 其实就是项目启动时加载 spring 的监听器，通过监听器和 dom4j 等解析 xml 技术读取 spring 的 xml 文件，把 spring.xml 文件中的所有bean标签读取到，拿到标签中的 class 属性，通过 java 的反射技术创建 bean 的实例，把所有创建好的 bean 实例放入一个 beanMap 中，beanMap 的 key 为 bean 标签的 ID 值，value 为 java 反射技术创建的具体实例对象，后续需要把创建好的 bean 注入给别的类使用时，其实就是通过 bean 的 id 属性从 beanMap 中获取对应 bean 实例，调用set方法把获取出来的 bean 注入到相关使用的实例中。)  




## spring概述 {id="spring_9"}
Spring就相当于一个粘合剂，有两个核心，一个核心是IOC (控制反转),它是基于工厂设计模式，所谓控制反转就是将自己手工完成对象创建(new)的这种任务交给spring容器去完成。和控制反转配套使用的还有一个DI也就是依赖注入。我们可以进行构造函数注入，属性注入等，最常用的还是属性注入。可以注入各种类型Map,List，properties。注入可以通过ByType和ByName分别按照类型和名字进行自动注入。Spring中的Bean支持单例和原型两种方式，默认是单例的。可以通过singleton=true/false来进行配置或者通过scope="singleton"，scope="prototype"来配置。所谓单例：即至始至终在jvm中都只有一个该类的实例。所谓原型：也叫多例，就每次都会创建一个新的对象实例。 

另一个核心是AOP(面向切面编程/面向方面编程),  AOP是OOP（面向对象编程）的延续，主要应用于日志记录，性能统计，安全控制,事务处理等方面。它是基于代理设计模式，而代理设计模式又分为静态代理和动态代理，静态代理比较简单就是一个接口，分别有一个真实实现和一个代理实现，而动态代理分为基于接口的jdk的动态代理和基于类的cglib的动态代理，Aop默认使用的是基于接口的jdk的动态代理。所谓动态代理，即通过代理类的代理，接口和实现类之间可以不直接发生联系，而可以在运行期（Runtime）实现动态关联。Jdk的动态代理要实现InvocationHandler接口并重写其中的invoke方法。

---

spring 就相当于一个粘合剂。 有两个核心，分别是IOC/DI(控制反转/依赖注入)。AOP(面向切面编程/面向方面编程)完全面向接口设计，主要用来进行bean实例的创建注入（IOC/DI），以及进行事务的管理（AOP）

---
Spring 是完全面向接口的设计，降低程序耦合性（为什么），主要是事务控制并创建bean实例对象。在ssh整合时，充当黏合剂的作用。IOC(Inversion of Control)控制反转/依赖注入，又称DI(Dependency Injection) (依赖注入) 从架构来讲默认注入组件都是单例，有效降低java 吃内存的问题。Spring大大降低了 java语言开发相互的内存。这是项目中使用spring的 主要原因。  
``IOC的作用``：产生对象实例，所以它是基于工厂设计模式的  
``Spring IOC的注入``：通过属性进行注入，通过构造函数进行注入，工厂注入  
``Spring IOC 自动绑定模式``：  
可以设置autowire按以下方式进行绑定。按byType只要类型一致会自动寻找，按byName自动按属性名称进行自动查找匹配。
AOP 面向方面（切面）编程。  
AOP是OOP的延续，是Aspect Oriented  Programming的缩写，意思是面向方面(切面)编程。（为啥是OOP的延续） 注：OOP(Object-Oriented Programming ) 面向对象编程   
AOP 主要应用于日志记录，性能统计，安全控制,事务处理（项目中使用的）等方面。  
``Spring中实现AOP技术``：  
在Spring中可以通过代理模式来实现AOP代理模式分为：   
``静态代理``：一个接口，分别有一个真实实现和一个代理实现。（代码 缺点）   
``动态代理``：通过代理类的代理，接口和实现类之间可以不直接发生联系，而可以在运行期（Runtime）实现动态关联。（优点）。动态代理有两种实现方式，可以通过jdk的动态代理实现也可以通过cglib 来实现而AOP默认是通过jdk的动态代理来实现的。jdk的动态代理必须要有接口的支持，而cglib不需要，它是基于类的。  

## spring有哪些特点，使用Spring有什么好处
1. 应用解耦
2. 依赖注入
3. AOP
4. 事务管理
5. MVC
6. 集成开发 

## spring应用程序看起来像什么 {id="spring_8"}
一些接口及其实现    
一些POJO类  
一些xml配置文件 

## spring核心容器模块是什么 {id="spring_7"}
Spring core/IOC/BeanFactory       
核心容器（Spring Core）   
核心容器提供Spring框架的基本功能。Spring以bean的方式组织和管理Java应用中的各个组件及其关系。Spring使用BeanFactory来产生和管理Bean，它是工厂模式的实现。BeanFactory使用控制反转(IoC)模式将应用的配置和依赖性规范与实际的应用程序代码分开。


## 为了降低Java开发的复杂性，Spring采取了哪几种策略
POJO/IOC/AOP/Template   
基于POJO的轻量性和最小侵入性编程    
通过依赖注入和面向接口实现松耦合    
基于切面和惯例进行声明式编程    
通过切面和模板减少样板式代码 



## spring如何处理线程并发问题 {id="spring_6"}
   
在同步机制中，通过对象的锁机制保证同一时间只有一个线程访问变量。这时该变量是多个线程共享的，使用同步机制要求程序慎密地分析什么时候对变量进行读写，什么时候需要锁定某个对象，什么时候释放对象锁等繁杂的问题，程序设计和编写难度相对较大。      
而ThreadLocal则从另一个角度来解决多线程的并发访问。ThreadLocal会为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。因为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。       
由于ThreadLocal中可以持有任何类型的对象，低版本JDK所提供的get()返回的是Object对象，需要强制类型转换。但JDK5.0通过泛型很好的解决了这个问题，在一定程度地简化ThreadLocal的使用。      
概括起来说，对于多线程资源共享的问题，同步机制采用了“以时间换空间”的方式，而ThreadLocal采用了“以空间换时间”的方式。前者仅提供一份变量，让不同的线程排队访问，而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响。 

Spring使用ThreadLocal解决线程安全问题  
我们知道在一般情况下，只有无状态的Bean才可以在多线程环境下共享，在Spring中，绝大部分Bean都可以声明为singleton作用域。就是因为Spring对一些Bean(如RequestContextHolder、TransactionSynchronizationManager、LocaleContextHolder等)中非线程安全状态采用ThreadLocal进行处理，让它们也成为线程安全的状态，因为有状态的Bean就可以在多线程中共享了。      
ThreadLocal和线程同步机制都是为了解决多线程中相同变量的访问冲突问题。

## Spring是如何控制线程安全的？ {id="spring_5"}
在同步机制中，通过对象的锁机制保证同一时间只有一个线程访问变量。这时该变量是多个线程共享的，使用同步机制要求程序慎密地分析什么时候对变量进行读写，什么时候需要锁定某个对象，什么时候释放对象锁等繁杂的问题，程序设计和编写难度相对较大。

而ThreadLocal则从另一个角度来解决多线程的并发访问。ThreadLocal会为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。因为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。

由于ThreadLocal中可以持有任何类型的对象，低版本JDK所提供的get()返回的是Object对象，需要强制类型转换。但JDK 5.0通过泛型很好的解决了这个问题，在一定程度地简化ThreadLocal的使用

概括起来说，对于多线程资源共享的问题，同步机制采用了“以时间换空间”的方式，而ThreadLocal采用了“以空间换时间”的方式。前者仅提供一份变量，让不同的线程排队访问，而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响。

Spring使用ThreadLocal解决线程安全问题

我们知道在一般情况下，只有无状态的Bean才可以在多线程环境下共享，在Spring中，绝大部分Bean都可以声明为singleton作用域。就是因为Spring对一些Bean（如RequestContextHolder、TransactionSynchronizationManager、LocaleContextHolder等）中非线程安全状态采用ThreadLocal进行处理，让它们也成为线程安全的状态，因为有状态的Bean就可以在多线程中共享了。

一般的Web应用划分为展现层、服务层和持久层三个层次，在不同的层中编写对应的逻辑，下层通过接口向上层开放功能调用。在一般情况下，从接收请求到返回响应所经过的所有程序调用都同属于一个线程ThreadLocal是解决线程安全问题一个很好的思路，它通过为每个线程提供一个独立的变量副本解决了变量并发访问的冲突问题。在很多情况下，ThreadLocal比直接使用synchronized同步机制解决线程安全问题更简单，更方便，且结果程序拥有更高的并发性。

如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的。 或者说:一个类或者程序所提供的接口对于线程来说是原子操作或者多个线程之间的切换不会导致该接口的执行结果存在二义性,也就是说我们不用考虑同步的问题。 　线程安全问题都是由全局变量及静态变量引起的。

若每个线程中对全局变量、静态变量只有读操作，而无写操作，一般来说，这个全局变量是线程安全的；若有多个线程同时执行写操作，一般都需要考虑线程同步，否则就可能影响线程安全。
1. 常量始终是线程安全的，因为只存在读操作。
2. 每次调用方法前都新建一个实例是线程安全的，因为不会访问共享的资源。
3. 局部变量是线程安全的。因为每执行一个方法，都会在独立的空间创建局部变量，它不是共享的资源。局部变量包括方法的参数变量和方法内变量。

有状态就是有数据存储功能。有状态对象(Stateful Bean)，就是有实例变量的对象   ，可以保存数据，是非线程安全的。在不同方法调用间不保留任何状态。

无状态就是一次操作，不能保存数据。无状态对象(Stateless Bean)，就是没有实例变量的对象   .不能保存数据，是不变类，是线程安全的。

有状态对象:

无状态的Bean适合用不变模式，技术就是单例模式，这样可以共享实例，提高性能。有状态的Bean，多线程环境下不安全，那么适合用Prototype原型模式。Prototype: 每次对bean的请求都会创建一个新的bean实例。

Struts2默认的实现是Prototype模式。也就是每个请求都新生成一个Action实例，所以不存在线程安全问题。需要注意的是，如果由Spring管理action的生命周期， scope要配成prototype作用域。

## spring框架中有哪些不同类型的事件 {id="spring_4"}

Spring的ApplicationContext 提供了支持事件和代码中监听器的功能。     
我们可以创建bean用来监听在ApplicationContext中发布的事件。ApplicationEvent类和在ApplicationContext接口中处理的事件，如果一个bean实现了ApplicationListener接口，当一个ApplicationEvent 被发布以后，bean会自动被通知。

```java
public class AllApplicationEventListener implements ApplicationListener < ApplicationEvent >{    
    @Override    
    public void onApplicationEvent(ApplicationEvent applicationEvent) {    
        //process event    
    }    
}
```

``Spring 提供了以下5中标准的事件：``

1. ``上下文更新事件（ContextRefreshedEvent）：``该事件会在ApplicationContext被初始化或者更新时发布。也可以在调用ConfigurableApplicationContext 接口中的refresh()方法时被触发。
2. ``上下文开始事件（ContextStartedEvent）：``当容器调用ConfigurableApplicationContext的Start()方法开始/重新开始容器时触发该事件。
3. ``上下文停止事件（ContextStoppedEvent）：``当容器调用ConfigurableApplicationContext的Stop()方法停止容器时触发该事件。
4. ``上下文关闭事件（ContextClosedEvent）：``当ApplicationContext被关闭时触发该事件。容器被关闭时，其管理的所有单例Bean都被销毁。
5. ``请求处理事件（RequestHandledEvent）：``在Web应用中，当一个http请求（request）结束触发该事件。    
除了上面介绍的事件以外，还可以通过扩展ApplicationEvent 类来开发自定义的事件。


```java
public class CustomApplicationEvent extends ApplicationEvent{    
    public CustomApplicationEvent ( Object source, final String msg ){    
        super(source);    
        System.out.println("Created a Custom event");    
    }    
}
```

为了监听这个事件，还需要创建一个监听器：

```java
public class CustomEventListener implements ApplicationListener < CustomApplicationEvent >{    
    @Override    
    public void onApplicationEvent(CustomApplicationEvent applicationEvent) {    
        //handle event    
    }    
}
```


之后通过applicationContext接口的 publishEvent()方法来发布自定义事件。

```java
CustomApplicationEvent customEvent = new CustomApplicationEvent(applicationContext, "Test message");    
applicationContext.publishEvent(customEvent);
```

## 什么是spring框架？spring 框架有哪些主要模块 {id="spring-spring_1"}
Spring 框架是一个为 Java 应用程序的开发提供了综合、 广泛的基础性支持的 Java 平台。Spring帮助开发者解决了开发中基础性的问题， 使得开发人员可以专注于应用程序的开发。 Spring 框架本身亦是按照设计模式精心打造， 这使得我们可以在开发环境中安心的集成 Spring 框架， 不必担心 Spring 是如何在后台进行工作的。Spring 框架至今已集成了 20 多个模块。 这些模块主要被分如下图所示的核心容器、 数据访问集成、Web、 AOP（ 面向切面编程）、 工具、 消息和测试模块。 



## 使用spring框架能带来哪些好处 {id="spring_2"}
下面列举了一些使用 Spring 框架带来的主要好处：
1.  DependencyInjection(DI) 方法使得构造器和 JavaBean properties 文件中的依赖关系一目了然。
2. 与 EJB 容器相比较，IOC 容器更加趋向于轻量级。 这样一来 IOC 容器在有限的内存和 CPU资源的情况下进行应用程序的开发和发布就变得十分有利。
3. Spring 并没有闭门造车，Spring 利用了已有的技术比如 ORM 框架、logging 框架、J2EE、Quartz 和 JDK Timer， 以及其他视图技术。
4. Spring 框架是按照模块的形式来组织的。 由包和类的编号就可以看出其所属的模块， 开发者仅仅需要选用他们需要的模块即可。
5. 要测试一项用 Spring 开发的应用程序十分简单， 因为测试相关的环境代码都已经囊括在框架中了。 更加简单的是， 利用 JavaBean 形式的 POJO 类， 可以很方便的利用依赖注入来写入测试数据。
6. Spring 的 Web 框架亦是一个精心设计的 Web MVC 框架， 为开发者们在 web 框架的选择上提供了一个除了主流框架比如 Struts、 过度设计的、 不流行 web 框架的以外的有力选项。
7. Spring 提供了一个便捷的事务管理接口， 适用于小型的本地事务处理（ 比如在单 DB 的环境下） 和复杂的共同事务处理（ 比如利用 JTA 的复杂 DB 环境）。

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

## springBoot定时器 {id="springboot_1"}
配置呢也很简单，只需要在启动类加上@EnableScheduling注解 在需要定时的方法中加上@Scheduled(cron=””)注解就可以啦 cron就是配置按时间执行方法的地方就可以啦。

## springBoot监控
springBoot中呢有一个叫spring-boot-admin的jar包用来做springBoot项目的监控，配置呢也很简单，只需要在监控的服务端项目中添加spring-boot-admin-server.jar和spring-boot-admin-server-ui.jar包就可以，然后在application.yml中配置spring.boot.admin.context-path 指明监控的服务地址，在main方法启动类上加上EnableAdminServer注解，这样服务端就配置好了，在需要被监控的项目中添加spring-boot-admin-starter-client.jar包，在application.yml配置文件中配置spring.boot.admin.url指明监控服务的地址，这样被监控端也就配置好了，然后我们访问监控服务在页面就可以看到被监控项目的内存使用情况，项目的健康情况，jvm的使用情况，网络路由情况，线程运行情况、还有数据源的一些信息等等。

## Spring填空题 {id="spring_1"}
1. spring中通过scope属性设置属性值为singleton来设置单例，设置属性值为prototype来设置多例（原型）。
2. bean注入时，引用数据类型通过ref属性或标签赋值。
3. spring控制事务的aop的切点应该控制在service层。
4. spring配置文件是applicationContext.xml。
5. aop配置事务。
   + read-only=true 配置只读事务
   + rollback-for   配置回滚策略
   + propagation    配置传播特性
   + transaction-manager    配置指定事务管理器
   + pointcut   定义AOP的切面
6. 定义切入点的表达式为：execution(* com.jk.*.service.*.*(..))     
   表示：com.jk.任意包.service.任意类.任意方法(..任意参数)execution(* )是固定格式
7. spring中自动注入的两种方式分别是byName和byType。
8. ioc的两种注入方式属性注入和构造方法注入。
9. Spring中的容器是ApplicationContext和BeanFactory。
