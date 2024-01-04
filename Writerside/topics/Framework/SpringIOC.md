# SpringIOC

## ioc的两种注入方式：属性注入、构造方法注入 {id="ioc_1"}

## IOC的理解 {id="ioc_2"}
1. IOC的作用：产生对象实例，所以它是基于工厂设计模式的
2. springIOC的注入：通过属性进行注入、通过构造函数进行注入。注入对象数组、注入List集合、注入Map集合、注入Properties类型
3. springIOC自动绑定模式：可以设置autowired按以下方式进行绑定按byType只要类型一致会自动寻找，按byName自动按属性名称进行自动查找匹配.


## spring实现ioc控制反转描述 {id="spring-ioc_1"}
原来需要我们自己进行bean的创建以及注入，而现在交给spring容器去完成bean的创建以及注入。所谓的“控制反转”就是 对象控制权的转移，从程序代码本身转移到了外部容器。

## springIOC原理
IOC(控制反转):对成员变量的赋值的控制权从代码中反转到配置文件中。
1. Spring中的org.springframework.beans包和org.springframework.context包构成了Spring框架IoC容器的基础。
2. BeanFactory接口提供了一个先进的配置机制，使得任何类型的对象的配置成为可能。ApplicationContext接口对BeanFactory（是一个子接口）进行了扩展，在BeanFactory的基础上添加了其他功能，比如与Spring的AOP更容易集成，也提供了处理message resource的机制（用于国际化）、事件传播以及应用层的特别配置，比如针对Web应用的WebApplicationContext。
3. org.springframework.beans.factory.BeanFactory是Spring IoC容器的具体实现，用来包装和管理前面提到的各种bean。BeanFactory接口是Spring IoC 容器的核心接口。

## spring IOC与DI的关系及区别
Ioc 是控制反转;原来需要我们自己进行bean的创建以及注入，而现在交给spring容器去完成bean的创建以及注入。所谓的“控制反转”就是 对象控制权的转移，从程序代码本身转移到了外部容器。       
DI：由容器动态的将某个依赖关系注入到组件之中。依赖注入的目的并非为软件系统带来更多功能，而是为了提升组件重用的频率，并为系统搭建一个灵活、可扩展的平台。

##  什么是控制反转(IOC)，什么是依赖注入
1. 控制反转是应用于软件工程领域中的， 在运行时被装配器对象来绑定耦合对象的一种编程技巧， 对象之间耦合关系在编译时通常是未知的。在传统的编程方式中，业务逻辑的流程是由应用程序中的早已被设定好关联关系的对象来决定的。在使用控制反转的情况下，业务逻辑的流程是由对象关系图来决定的， 该对象关系图由装配器负责实例化，这种实现方式还可以将对象之间的关联关系的定义抽象化。 而绑定的过程是通过“依赖注入” 实现的。
2. 控制反转是一种以给予应用程序中目标组件更多控制为目的设计范式， 并在我们的实际工作中起到了有效的作用。
3. 依赖注入是在编译阶段尚未知所需的功能是来自哪个的类的情况下， 将其他对象所依赖的功能对象实例化的模式。 这就需要一种机制用来激活相应的组件以提供特定的功能， 所以依赖注入是控制反转的基础。 否则如果在组件不受框架控制的情况下， 框架又怎么知道要创建哪个组件？
4. 在 Java 中依然注入有以下三种实现方式：
    1. 构造器注入
    2. Setter 方法注入
    3. 接口注入

## 请解释下spring框架中的IOC
Spring 中 的 org.springframework.beans 包和 org.springframework.context 包构成了Spring 框架 IOC 容器的基础。    
BeanFactory 接口 提供了一个先进的配置机制，使得任何类型的对象的配置成为可能。     
ApplicationContex 接口对 BeanFactory（是一个子接口）进行了扩展，在BeanFactory 的基础上添加了其他功能， 比如与 Spring 的 AOP 更容易集成， 也提供了处理 message resource的机制（用于国际化）、事件传播以及应用层的特别配置， 比如针对Web 应用WebApplicationContext。
