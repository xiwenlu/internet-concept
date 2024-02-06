# springConfig配置

## Spring提供几种配置方式来设置元数据？ {id="spring_1"}
将 Spring 配置到应用开发中有以下三种方式
1. 基于 XML 的配置（淘汰）
2. 基于注解的配置
3. 基于 Java 的配置，使xml中的bean转为JavaConfig文件方式

## 如何使用XML配置的方式配置Spring？
在 Spring框架中，依赖和服务需要在专门的配置文件来实现，我常用的XML格式的配置文件。这些配置文件的格式通常用开头，然后一系列的bean定义和专门的应用配置选项组成。

SpringXML配置的主要目的时候是使所有的 Spring 组件都可以用 xml 文件的形式来进行配置。这意味着不会出现其他的 Spring 配置类型，Spring 的 XML 配置方式是使用被 Spring 命名空间的所支持的一系列的 XML 标签来实现的。  

Spring的命名空间有：context、 beans、jdbc、 tx、 aop、 mvc 和 aso。

```xml

<beans>
    <bean class="org.springframework.web.servlet.view.BeanNameViewResolver"/>
    <bean class="org.springframework.web.servlet.view.json.MappingJackson2JsonView"/> 
</beans>

```

## 什么是Spring基于Java的配置？
Spring3.0之前都是基于XML配置的，Spring3.0开始可以几乎不使用XML而使用纯粹的java代码来配置Spring应用。Spring 对 Java 配置的支持是由@Configuration 注解和@Bean 注解来实现的。 

被@Configuration所注解的类则表示这个类的主要目的是作为 bean 定义的资源，声明的类可以通过在同一个类的内部调用。

@Bean 声明所起到的作用与元素类似，是来设置嵌入 bean 的依赖关系。由@Bean 注解的方法将会实例化、 配置和初始化一个新对象， 这个对象将由 Spring 的 IOC 容器来管理。  

```Java
// 第一种声明类
@Configuration
// com.jk 包首先会被扫到，然后再容器内查找被@Component 声明的类，找到后将这些类按照 SringBean 定义进行注册。
@ComponentScan(basePackages = "com.jk")
public class AppConfig{
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}

// 第二种声明类
// 可以配置Spring的Servlet监听器ContentLoaderListener 或 SpringMVC 的 DispatcherServlet。
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}

```


## FileSystemResource和ClassPathResource的区别
1. FileSystemResource：在环境变量中读取配置文件，需要给出spring-config.xml文件在你项目中的相对路径或者绝对路径。
2. ClassPathResource：在配置文件中读取配置文件，spring会在ClassPath中自动搜寻配置文件。如果配置文件位于src文件夹下，只需给出配置文件的名称，因为src文件夹是默认的类路径。
