# SpringBean

## springBean的生命周期 {id="springbean_1"}

Spring Bean的生命周期简单易懂。在一个bean实例被初始化时，需要执行一系列的初始化操作以达到可用的状态。同样的，当一个bean不在被调用时需要进行相关的析构操作，并从bean容器中移除。  
Spring bean factory 负责管理在 spring 容器中被创建的 bean 的生命周期。  

Bean 的生命周期由两组回调（callback） 方法组成。  
1. 初始化之后调用的回调方法。
2. 销毁之前调用的回调方法。

Spring 框架提供了以下四种方式来管理 bean 的生命周期事件： 
1. InitializingBean 和 DisposableBean 回调接口
2. 针对特殊行为的其他 Aware 接口
3. Bean 配置文件中的 Custom init()方法和 destroy()方法
4. @PostConstruct 和@PreDestroy 注解方式
   
使用 customInit()和 customDestroy()方法管理 bean 生命周期的代码样例如下：


```xml
<beans>
   <bean id="demoBean" class="com.leon.task.DemoBean"
         init-Method="customInit" destroy-Method="customDestroy">
   </bean>
</beans>
```

--- 

spring的核心是bean的依赖注入，注入到spring中的bean通常是以单例存在的，使用spring之后大大降低了java对内存的开销，避免了过多的对象创建，此处bean的生命周期是从tomcat初始化spring容器的时候创建，截止到tomcat关闭的时候bean销毁


--- 

<img src="bean.jpeg" alt=""/>



## springBean的五个作用域
Singleton/prototype/request/session        
1. singleton： 单例模式，在整个Spring IoC容器中，使用singleton定义的Bean将只有一个实例。    
2. prototype： 原型模式，每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新的Bean实例。    
3. request： 对于每次HTTP请求，使用request定义的Bean都将产生一个新实例，即每次HTTP请求将会产生不同的Bean实例。只有在Web应用中使用Spring时该作用域才有效。     
4. session： 对于每次HTTP Session，使用session定义的Bean都将产生一个新实例。同样只有在Web应用中使用Spring时该作用域才有效。
5. global-session：每个全局的HTTP Session，使用session定义的Bean都将产生一个新实例。

典型情况下，仅在使用portlet context的时候有效。同样只有在Web应用中使用Spring时，该作用域才有效其中比较常用的是singleton和 两种作用域。对于singleton作用域的Bean，每次请求该Bean都将获得相同的实例。容器负责跟踪Bean实例的状态，负责维护Bean实例的生命周期行为；如果一个Bean被设置成prototype作用域，程序每次请求该id的Bean，Spring都会新建一个Bean实例，然后返回给程序。在这种情况下，Spring容器仅仅使用new 关键字创建Bean实例，一旦创建成功，容器不在跟踪实例，也不会维护Bean实例的状态。

---

spring 容器中的 bean 可以分为 5 个范围。 所有范围的名称都是自说明的， 但是为了避免混淆，还是让我们来解释一下：
1. singleton： 这种 bean 范围是默认的， 这种范围确保不管接受到多少个请求， 每个容器中只
   有一个 bean 的实例， 单例的模式由 beanFactory 自身来维护。
2. prototype： 原形范围与单例范围相反， 为每一个 bean 请求提供一个实例。
3. request： 在请求 bean 范围内会每一个来自客户端的网络请求创建一个实例， 在请求完成以后， bean 会失效并被垃圾回收器回收。
4. Session： 与请求范围类似， 确保每个 session 中有一个 bean 的实例， 在 session 过期后，bean 会随之失效。
5. global-session：global-session 和 Portlet 应用相关。 当你的应用部署在 Portlet 容器中工作时， 它包含很多 portlet。 如果你想要声明让所有的 portlet 共用全局的存储变量的话， 那么这全局变量需要存储在 global-session 中。全局作用域与 Servlet 中的 session 作用域效果相同。


## spring框架中单例beans是线程安全的吗
不是线程安全（默认singleton）。当多个线程并发执行该请求对应的业务逻辑（成员方法），如果该处理逻辑中有对单例状态的修改（单例的成员属性），则必须考虑线程同步问题。

---


Spring框架并没有对单例bean进行任何多线程的封装处理。关于单例bean的线程安全和并发问题需要开发者自行去搞定。但实际上，大部分的Spring bean并没有可变的状态(比如Serview类和DAO类)，所以在某种程度上说Spring的单例bean是线程安全的。如果你的bean有多种状态的话（比如 View Model 对象），就需要自行保证线程安全。
最浅显的解决办法就是将多态bean的作用域由“singleton”变更为“prototype”。



## 哪些是最重要的bean生命周期方法？能重写它们吗？ {id="bean_1"}
Setup/teardown        
Xml中对应Init-method/destory-method       
@PostConstruct @PreDestory    
bean标签有两个重要的属性(init-method 和 destroy-method)，可以通过这两个属性定义自己的初始化方法和析构方法。Spring也有相应的注解：@PostConstruct 和 @PreDestroy。




## spring容器实例化Bean有多少种方法？分别是什么？
使用类构造器    
使用静态工厂方法    
Xml配置文件factory-method


## 什么是bean自动装配？并解释自动装配的各种模式？
Spring容器可以自动配置相互协作beans之间的关联关系。这意味着Spring可以自动配置一个bean和其他协作bean之间的关系，通过检查BeanFactory 的内容里没有使用和< property>元素。   
一个类就是一个Bean，Spring框架是一个Bean容器，替我们管理这些Bean，Spring就是来组织各个角色之间的关系，然后对这些角色进行调动。    
``自动装配：``
1. default（beans这个标签的default-autowired属性）
2. 通过byName自动装配
3. 通过byType自动装配
4. 通过构造函数自动装配
5. no不使用自动装配





## beanFactory和applicationContext有什么区别？


1. beanFactory采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化，这样我们就不能发现一些存在的spring的配置问题。
2. 而ApplicationContext则相反，它是在容器启动时，一次性创建了所有的单实列	Bean。这样在容器启动时，我们就可以发现Spring中存在的配置错误。因此 ApplicationContext	的初始化时间比BeanFactory稍长些，但访问bean的时候就更快些。

---

BeanFactory 可以理解为含有 bean 集合的工厂类。 BeanFactory 包含了种 bean 的定义， 以便在接收到客户端请求时将对应的 bean 实例化。     
BeanFactory 还能在实例化对象的时生成协作类之间的关系。 此举将 bean 自身与 bean 客户端的配置中解放出来。BeanFactory还包含了bean生命周期的控制，调用客户端的初始化方法（initialization methods） 和销毁方法（destruction methods）。从表面上看，application context 如同 bean factory 一样具有 bean 定义、 bean 关联关系的设置， 根据请求分发 bean 的功能。 但 application context 在此基础上还提供了其他的功能。
1. 提供了支持国际化的文本消息
2. 统一的资源文件读取方式
3. 已在监听器中注册的 bean 的事件

``以下是三种较常见的 ApplicationContext 实现方式：``
1. ClassPathXmlApplicationContext： 从 classpath 的 XML 配置文件中读取上下文， 并生成上下文定义。 应用程序上下文从程序环境变量中取得。
   ApplicationContext context = new ClassPathXmlApplicationContext(“application.xml”);
2. FileSystemXmlApplicationContext ： 由文件系统中的 XML 配置文件读取上下文。
   ApplicationContext context = new FileSystemXmlApplicationContext(“application.xml”);
3. XmlWebApplicationContext： 由 Web 应用的 XML 文件读取上下文。


## spring的两个容器ApplicationContext、BeanFactory？
1. ApplicationContext是基于BeanFactory的，BeanFactory就相当人的心脏，ApplicationContext就相当于人的躯干，Spring容器也是整个人，所以通常来说Spring的容器就是ApplicationContext。
2. BeanFactory是面向spring内部使用，ApplicationContext是面向使用spring框架的开发人员。

## 什么是spring inner beans
在Spring框架中，无论何时bean被使用时，当仅被调用了一个属性。一个明智的做法是将这个 bean 声明为内部 bean。内部bean可以用setter注入“属性”和构造方法注入“构造参数”的方式来实现。    
比如，在我们的应用程序中，一个Customer类引用了一个Person类，我们的要做的是创建一个 Person 的实例， 然后在 Customer 内部使用。

```java
public class Customer{
    private Person person;

    //Setters and Getters
}
public class Person{
    private String name;
    private String address;
    private int age;

    //Setters and Getters
}
```

内部 bean 的声明方式如下：
```xml
<bean id="CustomerBean" class="com.howtodoinjava.common.Customer">
    <property name="person">
        <!-- This is inner bean -->
        <bean class="com.howtodoinjava.common.Person">
            <property name="name" value="lokesh" />
            <property name="address" value="India" />
            <property name="age" value="34" />
        </bean>
    </property>
</bean>
```


