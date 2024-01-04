# spring模块

## Spring的7大模块
1. 核心模块(Spring core):
   Spring Core模块是Spring的核心容器，它实现了IOC模式，提供了Spring框架的基础功能。此模块中包含的BeanFactory类是Spring的核心类，负责JavaBean的配置与管理。它采用Factory模式实现了IOC即依赖注入。谈到JavaBean，它是一种 Java 类，它遵从一定的设计模式，使它们易于和其他开发工具和组件一起使用。定义 JavaBean 是一种JAVA 语言写成的可重用组件。要编写JavaBean，类必须是具体类和公共类，并且具有无参数的构造器。
2.  Spring Context模块:
    Spring Context模块继承BeanFactory（或者说Spring核心）类，并且添加了事件处理、国际化、资源装载、透明装载、以及数据校验等功能。它还提供了框架式的Bean的访问方式和很多企业级的功能，如JNDI访问、支持EJB、远程调用、集成模板框架、Email和定时任务调度等。
3. Spring AOP模块:
   Spring集成了所有AOP功能。通过事务管理可以使任意Spring管理的对象AOP化。Spring提供了用标准Java语言编写的AOP框架，它的大部分内容都是基于AOP联盟的API开发的。它使应用程序抛开EJB的复杂性，但拥有传统EJB的关键功能。
4. Spring DAO模块:
   DAO是 Data Access Object的缩写，DAO模式思想是将业务逻辑代码与数据库交代码分离，降低两者耦合。通过DAO模式可以使结构变得更为清晰，代码更为简洁。DAO模块提供了JDBC的抽象层，简化了数据库厂商的异常错误（不再从SQLException继承大批代码），大幅度减少代码的编写，并且提供了对声明式事务和编程式事务的支持。
5. Spring ORM映射模块:
   Spring ORM 模块提供了对现有ORM框架的支持，各种流行的ORM框架已经做得非常成熟，并且拥有大规模的市场，Spring没有必要开发新的ORM工具，它对Hibernate提供了完美的整合功能，同时也支持其他ORM工具。注意这里Spring是提供各类的接口（support），目前比较流行的下层数据库封闭映射框架，如 ibatis, Hibernate等。
6. Spring Web模块:
   此模块建立在Spring Context 基础之上，它提供了Servlet监听器的Context 和 Web应用的上下文。对现有的Web框架，如JSF、Tapestry、Structs等，提供了集成。Structs是建立在MVC 这种公认的好的模式上的，Struts在M、V和C上都有涉及，但它主要是提供一个好的控制器和一套定制的标签库上，也就是说它的着力点在C和V上，因此，它天生就有MVC所带来的一系列优点，如：结构层次分明，高可重用性，增加了程序的健壮性和可伸缩性，便于开发与设计分工，提供集中统一的权限控制、校验、国际化、日志等等。
7. Spring Web MVC模块:
   spring WebMVC模块建立在Spring核心功能之上，这使它能拥有Spring框架的所有特性，能够适应多种多视图、模板技术、国际化和验证服务，实现控制逻辑和业务逻辑的清晰分离。说说MVC在JSP的作用，这里引入了“控制器”这个概念，控制器一般由Servlet来担任，客户端的请求不再直接送给一个处理业务逻辑的JSP页面，而是送给这个控制器，再由控制器根据具体的请求调用不同的事务逻辑，并将处理结果返回到合适的页面。因此，这个Servlet控制器为应用程序提供了一个进行前-后端处理的中枢。一方面为输入数据的验证、身份认证、日志及实现国际化编程提供了一个合适的切入点；另一方面也提供了将业务逻辑从JSP文件剥离的可能。业务逻辑从JSP页面分离后，JSP文件蜕变成一个单纯完成显示任务的东西，这就是常说的View。而独立出来的事务逻辑变成人们常说的Model，再加上控制器Control本身，就构成了MVC模式。实践证明，MVC模式为大型程序的开发及维护提供了巨大的便利。


