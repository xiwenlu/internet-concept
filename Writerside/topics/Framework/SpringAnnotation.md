# spring注解  

## annotation之@Autowired、@Inject、@Resource三者区别
1. @Autowired是spring自带的，@Inject是JSR330规范实现的，@Resource是JSR250规范实现的，需要导入不同的包
2. @Autowired、@Inject用法基本一样，不同的是@Autowired有一个request属性
3. @Autowired、@Inject是默认按照类型匹配的，@Resource是按照名称匹配的
4. @Autowired如果需要按照名称匹配需要和@Qualifier一起使用，@Inject和@Name一起使用

## @RequestPart和@RequestParam的区别
使用multipart/form-data编码类型向后端同时传文件和JSON等其他类型数据时会用到@RequestPart注解，这里粗略的说一下两者的区别（语言上描述不够严谨）
1. @RequestPart注解的MultipartFile类型参数使用MultipartResolver关联，其它的类型的参数使用HttpMessageConverter（类似@RequestBody）
2. @RequestParam注解的非String或MultipartFile/Part类型，转换器需要一个注册的Converter 或 PropertyEditor


## spring的常见注解 {id="spring_1"}
1. @service 声明是业务层，创建bean实例。   
2. @Repository 声明是持久层，创建bean实例。     
3. @autowired 自动注入默认的是按byType类型注入。

## spring注解的基本概念和原理
注解(Annotation)，也叫元数据。一种代码级别的说明。    
``Spring注解分为：``
1. 类级别的注解：如@Component、@Repository、@Controller、@Service以及JavaEE6的@ManagedBean和@Named注解，都是添加在类上面的类级别注解。    
   Spring容器根据注解的过滤规则扫描读取注解Bean定义类，并将其注册到Spring IoC容器中。
2. 类内部的注解：如@Autowire、@Value、@Resource以及EJB和WebService相关的注解等，都是添加在类内部的字段或者方法上的类内部注解。    
   SpringIoC容器通过Bean后置注解处理器解析Bean内部的注解。


## 什么是基于注解的容器配置
XML解耦了配置和原代码，而注解则精简了配置。spring框架基于注解的容器配置：   
1. @Qualifier  
2. @Autowired  
3. @Resource   
4. @PostContuct    
5. @PreDestory

## @Autowired @Resource @Inject 的区别
@Resource
1. @Resource是JSR250规范的实现，需要导入javax.annotation实现注入。
2. @Resource是根据名称进行自动装配的，一般会指定一个name属性
3. @Resource可以作用在变量、setter方法上。

@Autowired
1. @Autowired是spring自带的，@Inject是JSR330规范实现的，@Resource是JSR250规范实现的，需要导入不同的包
2. @Autowired、@Inject用法基本一样，不同的是@Autowired有一个request属性
3. @Autowired、@Inject是默认按照类型匹配的，@Resource是按照名称匹配的
4. @Autowired如果需要按照名称匹配需要和@Qualifier一起使用，@Inject和@Name一起使用

## 请举例解释@Autowired注解

@Autowired注解对自动装配何时何处被实现提供了更多细粒度的控制。@Autowired注解可以像@Required注解、构造器一样被用于在bean的设值方法上自动装配bean的属性，一个参数或者带有任意名称或带有多个参数的方法。        
比如，可以在设值方法上使用@Autowired注解来替代配置文件中的\<property>元素。当Spring容器在setter方法上找到@Autowired注解时，会尝试用byType 自动装配。     
当然我们也可以在构造方法上使用@Autowired注解。带有@Autowired注解的构造方法意味着在创建一个bean时将会被自动装配，即便在配置文件中使用\<constructor-arg> 元素。

```java
public class TextEditor {    
   private SpellChecker spellChecker;    
   @Autowired    
   public TextEditor(SpellChecker spellChecker){    
      System.out.println("Inside TextEditor constructor." );    
      this.spellChecker = spellChecker;    
   }    
   public void spellCheck(){    
      spellChecker.checkSpelling();    
   }    
}
```

下面是没有构造参数的配置方式：

```xml
<beans>    
     
   <context:annotation-config/>    
     
   <!-- Definition for textEditor bean without constructor-arg  -->    
   <bean id="textEditor" class="com.howtodoinjava.TextEditor"/>    
     
   <!-- Definition for spellChecker bean -->    
   <bean id="spellChecker" class="com.howtodoinjava.SpellChecker"/>    
     
</beans>
```



## 请举例说明@Qualifier注解

@Qualifier注解意味着可以在被标注bean的字段上可以自动装配。Qualifier注解可以用来取消Spring不能取消的bean应用。  
下面的示例将会在Customer的person属性中自动装配person的值。

```java
public class Customer{    
    @Autowired    
    private Person person;    
}
```

下面我们要在配置文件中来配置Person类。

```xml
<bean id="personA" class="com.somnus.common.Person" >    
    <property name="name" value="lokesh" />    
</bean>    
     
<bean id="personB" class="com.somnus.common.Person" >    
    <property name="name" value="alex" />    
</bean>
```

Spring会知道要自动装配哪个person bean么？不会的，但是运行上面的示例时，会抛出下面的异常：

```
Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException:    
    No unique bean of type [com.howtodoinjava.common.Person] is defined:    
        expected single matching bean but found 2: [personA, personB]
```

要解决上面的问题，需要使用  @Quanlifier注解来告诉Spring容器要装配哪个bean：

```
public class Customer{    
    @Autowired    
    @Qualifier("personA")    
    private Person person;    
}
```
