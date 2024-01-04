# SpringAOP

## AOP的理解 {id="aop_1"}
**概念解释**   
1. 切面(Aspect)：有切点(PointCut)和通知(Advice)组成，它既包括横切逻辑的定义，也包括了连接点的定义。   
2. 切点(Pointcut)： 一个切点定位多个类中的多个方法。   
3. 通知也叫增强(Advice)： 由方位和横切逻辑构成,所谓的方位指的是前置通知，后置通知，返回后通知，环绕通知，抛出异常后通知   
4. 连接点(JoinPoint)：由切点和方位构成，用来描述在在哪些类的指定方法之前或之后执行  

**所谓的方位包括**   
1. 前置通知（Before advice）：在连接点（join point）之前执行的通知，但这个通知不能阻止连接点前的执行（除非它抛出一个异常）。   
2. 返回后通知（After returning advice）：在连接点（join point）正常完成后执行的通知：例如，一个方法没有抛出任何异常，正常返回。
3. 抛出异常后通知（After throwing advice）：在方法抛出异常退出时执行的通知。   
4. 后置通知（After (finally)advice）：当连接点退出的时候执行的通知（不论是正常返回还是异常退出）。   
5. 环绕通知（Around Advice）：包围一个连接点（join point）的通知，如方法调用。这是最强大的一种通知类型。环绕通知可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它们自己的返回值或抛出异常来结束执行

## AOP的理解 {id="aop_2"}
1. AOP是OOP（面向对象编程）的延续，是Aspect Oriented Programming的缩写，意思是面向方面编程或者面向切面编程。
2. AOP主要应用于日志记录，性能统计，安全控制，事务处理等方面。它的主要意图就要将日志记录，性能统计，安全控制等等代码从核心代码中清楚的划分出来。
3. AOP代理可以通过jdk动态代理实现，也可以通过cglib实现，默认是通过jdk动态代理实现的。jdk动态代理需要接口的支持，如果没有接口只有类，则使用cglib来实现。


## springAOP事务的描述
在spring-common.xml里通过\<aop:config>里面先设定一个表达式，设定对service里那些方法 如：对add* ,delete*,update*等开头的方法进行事务拦截。我们需要配置事务的传播（propagation="REQUIRED"）特性,通常把增,删,改以外的操作需要配置成只读事务（read-only="true"）.只读事务可以提高性能。之后引入tx:advice,在tx:advice引用 transactionManager（事务管理）,在事务管理里再引入sessionFactory,sessionFactory注入 dataSource，最后通过\<aop:config> 引入txAdvice。


## springAOP作用及原理 {id="springaop_2"}
``Aop作用：``AOP 主要应用于日志记录，性能统计，安全控制,事务处理（项目中使用的）等方面。      
``原理：``      
AOP 面向方面（切面）编程      
AOP是OOP的延续，是Aspect Oriented Programming的缩写，意思是面向方面(切面)编程。（为啥是OOP的延续） 注：OOP(Object-Oriented Programming ) 面向对象编程      
AOP 主要应用于日志记录，性能统计，安全控制,事务处理（项目中使用的）等方面。      
``Spring中实现AOP技术：``       
``在Spring中可以通过代理模式来实现AOP代理模式分为：``     
``静态代理：``一个接口，分别有一个真实实现和一个代理实现。（代码 缺点）       
``动态代理：``通过代理类的代理，接口和实现类之间可以不直接发生联系，而可以在运行期（Runtime）实现动态关联。（优点）。动态代理有两种实现方式，可以通过jdk的动态代理实现也可以通过cglib 来实现而AOP默认是通过jdk的动态代理来实现的。jdk的动态代理必须要有接口的支持，而cglib不需要，它是基于类的。         
``Aop 对事物处理的原理``        
在spring-common.xml里通过\<aop:config>里面先设定一个表达式，设定对service里那些方法 如：对add* ,delete*,update*等开头的方法进行事务拦截。我们需要配置事务的传播（propagation="REQUIRED"）特性,通常把增,删,改以外的操作需要配置成只读事务（read-only="true"）.只读事务可以提高性能。之后引入tx:advice,在tx:advice引用 transactionManager（事务管理）,在事务管理里再引入sessionFactory,sessionFactory注入 dataSource，最后通过\<aop:config> 引入txAdvice。

## 使用springAOP实现日志管理功能简要说一下思路 {id="springaop_1"}
1. 在spring配置文件中配置aop切面，具体是通过\<bean>标签制定具体切面类 通过pointcut 配置切点表达式。在advice-ref通知器制定切面
2. 在切面类中我们要实现后置通知的接口及其方法，因为我们要将操作记录存到mongodb数据库中所以在切面类里首先注入mongoTemplate，在实现的方法中将方法的相关参数包括方法的方法名，内路径，返回值，参数 set到对应的实体对象中。最后调用mongoTemplate的insert()方法将实体对象存到mongodb数据库中


## 什么是AOP，有什么作用，能应用在什么场景 {id="aop_3"}
面向切面编程。    
``AOP主要作用:``将日志记录，性能统计，安全控制，事务处理，异常处理等代码从业务逻辑代码中划分出来，通过对这些行为的分离，我们希望可以将它们独立到非指导业务逻辑的方法中，进而改变这些行为的时候不影响业务逻辑的代码。  
总之，面向切面的目标与面向对象的目标没有不同。    
一是减少重复，二是专注业务。  
AOP应用场景:日志记录，性能统计，安全控制，事务处理，异常处理等问题及扩展


## 什么是织入，织入的时机是什么
把切面（aspect）连接到其它的应用程序类型或者对象上，并创建一个被通知（advised）的对象，这样的行为叫做织入。  
==织入操作可以发生在如下几个阶段。==
``编译时：``在对源代码进行编译时，特殊的编译器允许我们通过某种方式指定程序中的各个方面进行Weave的规则，并根据这些规则生成编译完成的应用程序；  
``编译后：``根据Weave规则对已经完成编译的程序模块进行Weave操作；    
``载入时：``在载入程序模块的时候进行Weave操作；     
``运行时：``在程序运行时，根据情况织入程序中的对象和方面。

编译期织入是指在Java编译期，将切面织入到Java类中；而类加载期织入则指通过特殊的类加载器，在类字节码加载到JVM时，织入切面；运行期织入则是采用CGLib工具或JDK动态代理进行切面的织入。


<table border="1" cellspacing="0" cellpadding="0">
    <tr>
        <td>织入时机</td>
        <td>AspectJ</td>
        <td>Spring</td>
        <td>JBoss</td>
        <td>AspectC</td>
    </tr>
    <tr>
        <td>编译时</td>
        <td>√</td>
        <td></td>
        <td></td>
        <td>√</td>
    </tr>
    <tr>
        <td>编译后</td>
        <td>√</td>
        <td></td>
        <td></td>
        <td>√</td>
    </tr>
    <tr>
        <td>载入时</td>
        <td>√</td>
        <td>√</td>
        <td>√</td>
        <td></td>
    </tr>
    <tr>
        <td>运行时</td>
        <td></td>
        <td></td>
        <td>√</td>
        <td></td>
    </tr>
</table>


## 什么是切入点，关注点，连接点
``连接点（JoinPoint）`` spring允许你是通知（Advice）的地方，基本每个方法的前、后、环绕或抛出异常时都可以是连接点，spring只支持方法连接点。其他如AspectJ还可以让你在构造器或属性注入时都行。        
``切入点（Pointcut）`` 一个类里，有N个方法就有N个连接点，但是你并不想在所有方法上都使用通知，只想让其中几个方法执行前、后或者抛出异常完成其他功能（如日志，性能分析），那么就用切入点来筛选到那几个你想要的方法。


```java
public class BusinessLogic {
    public void doSomething() {
        // 验证安全性；Securtity关注点
        // 执行前记录日志；Logging关注点
        doit();
        // 保存逻辑运算后的数据；Persistence关注点
        // 执行结束记录日志；Logging关注点
    }
}
```
Business Logic属于核心关注点，它会调用到Security，Logging，Persistence等横切关注点。


## spring提供了几种AOP支持
``方式一：``经典的基于代理的AOP     
``方式二：``@AspectJ注解的切面   
``方式三：``纯POJO切面      
AOP常用的``实现方式``有两种，   
一种是采用声明的方式来实现（基于XML），     
一种是采用注解的方式来实现（基于AspectJ）。





## 谈谈你对spring框架的理解AOP IOC 事务怎么配置的
在我平常的工作过程中对spring的应用还是比较多的，整体感觉也没有什么，主要有两个核心AOP和IOC/DI，分别为面向切面编程思想和控制反转/依赖注入，工作过程中主要用到IOC注入这块比较多，通过spring的注入能够更加方便维护bean之间的关系，大多数的bean实例都是单例的，很好的节省的对内存的消耗。Spring的注入方式主要用到过构造函数注入，属性注入，注入数据的类型比较多，常用对象注入，也使用到过list集合注入，使用比较灵活。      
ioc是控制反转，主要用来维护bean之间的注入关系，使用工厂模式创建bean的实例，然后再根据xml中配置的bean的注入关系为创建好的实例注入bean对象。       
Spring的aop面向切面编程是基于代理模式实现的，通过我的工作经验来看，它其实就是把一部分公用的代码段提取到一个实现类中，让程序在运行过程中把代码再拼接成完整代码执行，增强代码的可读性和高复用性，尽可能的避免重复代码的出现，但是并不是所有的业务都适合使用spring的aop功能。        
Spring的aop由切点和通知组成，通知分为前置通知、后置通知和环绕通知，我这块主要用过前置和后置通知。环绕通知这块大概了解过，没有深入的理解，主要把公用代码提取到前置通知和后置通知中，在代码运行过程中先运行前置通知方法，再运行切点方法，最后运行后置通知方法。      
==注释:java反射技术可以通过类的全限定名创建对应对象，创建对象的方法是newInstance创建对应对象。==      
JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。      
spring原理这块我也有一定的了解，使用spring这么长时间，我认为spring其实就是项目启动时加载spring的监听器，通过监听器和dom4j等解析xml技术读取spring的xml文件，把spring.xml文件中的所有bean标签读取到，拿到标签中的class属性，通过java的反射技术创建bean的实例，把所有创建好的bean实例放入一个beanMap中，beanMap的key为bean标签的ID值，value为java反射技术创建的具体实例对象，后续需要把创建好的bean注入给别的类使用时，其实就是通过bean的id属性从beanMap中获取对应bean实例，调用set方法把获取出来的bean注入到相关使用的实例中。





## 过滤器、拦截器、AOP的区别

1. 过滤器   
   过滤器可以拦截到方法的请求和响应（ServletRequest request, ServletResponse response），并对请求响应做出响应的过滤操作，比如设置字符编码、鉴权操作。
2. 拦截器  
   拦截器可以在方法之前（preHandle）和方法执行之后（afterCompletion）进行操作，回调操作（postHandle），可以获取执行的方法的名称，请求（HttpServletRequest）。
3. AOP切片    
   AOP操作可以对操作进行横向的拦截，最大的优势在于可以获取执行方法的参数，对方法进行统一的处理，常见使用日志，事务，请求参数安全验证等。
   
