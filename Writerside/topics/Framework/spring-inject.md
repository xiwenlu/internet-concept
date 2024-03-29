# spring注入

1. spring有byName和byType两种自动注入方式。
2. 在Spring中可以注入null或空字符串。
3. 不是所有类型都能自动装配，原生类型和字符串类型不可以自动装配。

## 请举例说明如何在 Spring 中注入一个 Java 集合
Spring 提供了以下四种集合类的配置元素：
1. 该标签用来装配可重复的 list 值。
2. 该标签用来装配没有重复的 set 值。
3. 该标签可用来注入键和值可以为任何类型的键值对。
4. 该标签支持注入键和值都是字符串类型的键值对。     
   下面看一下具体的例子：

## 如何向 Spring Bean 中注入 java.util.Properties
``第一种方法是使用如下面代码所示的 标签：``     
admin@gupaoedu.com  
support@gupaoedu.com    
也可用” util:” 命名空间来从 properties 文件中创建出一个 propertiesBean， 然后利用 setter方法注入 bean 的引用。

## 请解释SpringBean的自动装配
在 Spring 框架中， 在配置文件中设定 bean 的依赖关系是一个很好的机制，Spring 容器还可以自动装配合作关系 bean 之间的关联关系。 这意味着 Spring 可以通过向 Bean Factory 中注入的方式自动搞定 bean 之间的依赖关系。 自动装配可以设置在每个 bean 上， 也可以设定在特定的 bean 上。    
``下面的 XML 配置文件表明了如何根据名称将一个 bean 设置为自动装配：``

除了 bean 配置文件中提供的自动装配模式， 还可以使用@Autowired 注解来自动装配指定的bean。 在使用@Autowired 注解之前需要在按照如下的配置方式在 Spring 配置文件进行配置才可以使用。        
也可以通过在配置文件中配置 AutowiredAnnotationBeanPostProcessor 达到相同的效果。

```xml
="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/>
```

配置好以后就可以使用@Autowired 来标注了。

```java
@Autowired
public EmployeeDAOImpl ( EmployeeManager manager ) {
    this.manager = manager;
}
```

## 请解释各种自动装配模式的区别
在 Spring 框架中共有 5 种自动装配， 让我们逐一分析。
1. no： 这是 Spring 框架的默认设置， 在该设置下自动装配是关闭的， 开发者需要自行在 bean定义中用标签明确的设置依赖关系。
2. byName：该选项可以根据 bean 名称设置依赖关系。 当向一个 bean 中自动装配一个属性时，容器将根据 bean 的名称自动在在配置文件中查询一个匹配的 bean。 如果找到的话， 就装配这个属性， 如果没找到的话就报错。
3. byType： 该选项可以根据 bean 类型设置依赖关系。 当向一个 bean 中自动装配一个属性时，容器将根据 bean 的类型自动在在配置文件中查询一个匹配的 bean。 如果找到的话， 就装配这个属性， 如果没找到的话就报错。
4. constructor： 造器的自动装配和 byType 模式类似， 但是仅仅适用于与有构造器相同参数的bean， 如果在容器中没有找到与构造器参数类型一致的 bean， 那么将会抛出异常。
5. autodetect： 该模式自动探测使用构造器自动装配或者 byType 自动装配。 首先， 首先会尝试找合适的带参数的构造器， 如果找到的话就是用构造器自动装配， 如果在 bean 内部没有找到相应的构造器或者是无参构造器， 容器就会自动选择 byTpe 的自动装配方式。


## 如何开启基于注解的自动装配
要使用 @Autowired， 需要注册 AutowiredAnnotationBeanPostProcessor， 可以有以下两
种方式来实现：
1. 引入配置文件中的下引入 <context:annotation-config>

```xml
<beans>    
    <context:annotation-config />    
</beans>  
```

2. 在 bean 配置文件中直接引入 AutowiredAnnotationBeanPostProcessor

```xml
<beans>    
    <bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/>    
</beans> 
```

## 自动装配有哪些局限性
``自动装配有如下局限性：``  
``重写：`` 你仍然需要使用 和< property>设置指明依赖， 这意味着总要重写自动装配。    
``原生数据类型:``你不能自动装配简单的属性， 如原生类型、 字符串和类。   
``模糊特性：`` 自动装配总是没有自定义装配精确， 因此， 如果可能尽量使用自定义装配。





## 构造方法注入和设值注入有什么区别

``请注意以下明显的区别：``

1. 在设值注入方法支持大部分的依赖注入，如果我们仅需要注入int、string和long型的变量，我们不要用设值的方法注入。对于基本类型，如果我们没有注入的话，可以为基本类型设置默认值。在构造方法注入不支持大部分的依赖注入，因为在调用构造方法中必须传入正确的构造参数，否则的话为报错。
2. 设值注入不会重写构造方法的值。如果我们对同一个变量同时使用了构造方法注入又使用了设置方法注入的话，那么构造方法将不能覆盖由设值方法注入的值。很明显，因为构造方法尽在对象被创建时调用。
3. 在使用设值注入时有可能还不能保证某种依赖是否已经被注入，也就是说这时对象的依赖关系有可能是不完整的。而在另一种情况下，构造器注入则不允许生成依赖关系不完整的对象。
4. 在设值注入时如果对象A和对象B互相依赖，在创建对象A时Spring会抛出sObjectCurrentlyInCreationException异常，因为在B对象被创建之前A对象是不能被创建的，反之亦然。所以Spring用设值注入的方法解决了循环依赖的问题，因对象的设值方法是在对象被创建之前被调用的。


## 自动装配有哪些好处和坏处
``自动装配的优点如下：``
1. 自动装配能显著减少装配的数量，因此在配置数量相当多时采用自动装配，可以减少工作量。
2. 自动装配可以使配置与Java代码同步更新。例如：如果需要给一个Java类增加一个依赖，那么该依赖将自动实现而不需要修改配置。因此强烈在开发过程中采用自动装配，而在系统趋于稳定的时候改为显式装配的方式。

虽然自动装配具有上面这些优点，但不是说什么时候都可以使用它，因为它还有如下一些``缺点``：
1. 尽管自动装配比显式装配更神奇，但是，Spring会尽量避免在装配不明确时进行猜测，因为装配不明确可能出现难以预料的结果，而Spring所管理的对象之间的关联关系也不再能清晰地进行文档化。
2. 对于那些根据Spring配置文件生成文档的工具来说，自动装配将会使这些工具无法生成依赖信息。




## 什么是循环依赖
循环依赖就是N个类中循环嵌套引用，如果在日常开发中我们用new对象的方式发生这种循环依赖的话程序会在运行时一直循环调用，直至内存溢出报错。

```java
public class PersonA {
    private PersonB personB;
    public PersonA(PersonB personB) {
        this.personB = personB;
    }
}
public class PersonB {
    private PersonC personC;
    public PersonB(PersonC personC) {
        this.personC = personC;
    }
}
public class PersonC {
    private PersonA personA;
    public PersonC(PersonA personA) {
        this.personA = personA;
    }
}
```

## Spring如何解决循环依赖
==DefaultSingletonBeanRegistry.getSingleton源码中有3个cache：==
``singletonFactories：`` 单例对象工厂的cache  
``earlySingletonObjects：``提前暴光的单例对象的Cache    
``singletonObjects：``单例对象的cache

getSingleton()的整个过程，Spring首先从一级缓存singletonObjects中获取。如果获取不到，并且对象正在创建中，就再从二级缓存earlySingletonObjects中获取。如果还是获取不到且允许singletonFactories通过getObject()获取，就从三级缓存singletonFactory.getObject()（三级缓存）获取。    
``解析：``      
PersonA的setter依赖了PersonB的实例对象，同时PersonB的setter依赖了PersonA的实例对象”这种循环依赖的情况。     
PersonA首先完成了初始化的第一步，并且将自己提前曝光到singletonFactories中，此时进行初始化的第二步，发现自己依赖对象PersonB，此时就尝试去get(PersonB)，发现PersonB还没有被create，所以执行create流程，PersonB在初始化第一步的时候发现自己依赖了对象PersonA，于是尝试get(PersonA)，尝试一级缓存singletonObjects(肯定没有，因为A还没初始化完全)，尝试二级缓存earlySingletonObjects（也没有），尝试三级缓存singletonFactories，由于PersonA通过ObjectFactory将自己提前曝光了，所以PersonB能够通过ObjectFactory.getObject拿到PersonA对象(虽然PersonA还没有初始化完全)，PersonB拿到PersonA对象后顺利完成了初始化阶段1、2、3，完全初始化之后将自己放入到一级缓存singletonObjects中。此时返回PersonA中，PersonA此时能拿到PersonB的对象顺利完成自己的初始化阶段2、3，最终A也完成了初始化，进去了一级缓存singletonObjects中。而且，由于PersonB拿到了PersonA的对象引用，所以PersonB现在中的PersonA对象完成了初始化。

## 怎样用注解的方式配置 Spring？ {id="spring_2"}
Spring在2.5版本以后开始支持用注解的方式来配置依赖注入。可以用注解的方式来替代XML方式的 bean 描述，可以将bean描述转移到组件类的内部，只需要在相关类上、方法上或者字段声明上使用注解即可。注解注入将会被容器在XML注入之前被处理，所以后者会覆盖掉前者对于同一个属性的处理结果。注解装配在Spring中是默认关闭的。所以需要在Spring文件中配置一下才能使用基于注解的装配模式。如果你想要在你的应用程序中使用关于注解的方法的话， 请参考如下的配置。   
在标签配置完成以后，就可以用注解的方式在Spring中向属性、方法和构造方法中自动装配变量。      
``下面是几种比较重要的注解类型：``
1. ``@Required：`` 该注解应用于设值方法。
2. ``@Autowired：`` 该注解应用于有值设值方法、 非设值方法、 构造方法和变量。
3. ``@Qualifier：`` 该注解和@Autowired 注解搭配使用， 用于消除特定 bean 自动装配的歧义。
4. ``JSR-250 Annotations：`` Spring 支 持 基 于 JSR-250 注 解 的 以 下 注 解 ， @Resource、@PostConstruct 和 @PreDestroy。