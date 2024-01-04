# springMVC


## springMVC和spring的关系
springMVC位于spring web端的一个框架，是一种基于Java的实现了Web MVC设计模式的请求驱动类型的轻量级Web框架，即使用了MVC架构模式的思想，将web层进行职责解耦。附：基于请求驱动指的就是使用请求-响应模型。  
从名字上就可以窥探出，Spring>SpringMVC，那么事实上，spring和SpringMVC是一种父子关系。SpringMVC是spring扩展出的一个应用于web端的框架。在这里需要注意的一点，就是到底什么是父子容器关系：    
spring主要的作用是黏合其他模块组件，进行统一管理，springmvc则主要是负责web端。那么，我们都知道，我们在应用spring的时候，可以使用注入。这个时候，如果我们的web端是用的SpringMVC，这个时候，controller理论上是通过SpringMVC去注入，但是，使用spring注入，同样是可行的。同理，service等层，使用SpringMVC配置的统一扫描装配也是可以的。所以，如果说只是为了使用spring的依赖注入，是大可不必将springMVC和spring同时使用的。他们完全可以分开！   
但是，尽管SpringMVC和spring都可以进行自动装配扫描，值得注意的是：    
spring（父容器）并不能直接访问SpringMVC（子容器）所注入的对象，但是SpringMVC却可以访问到spring装载的对象。所以，在配置自动装配的时候，应该注意到这一点。

## springMVC、struts1与struts2的主要区别
1. springMVC单例 非线程安全  
    struts1单例 非线程安全    
    struts2线程安全对每个请求都产生一个实例
2. springMVC的入口是servlet，而struts2是filter    
    1. spring 的前端总控制器为 DispatcherServlet
    2. struts2 的前端总控制器为 FilterDispatcher  
    3. struts1 的前端总控制器为 actionServlet
3. 参数传递
    1. struts是在接受参数的时候，可以用属性来接受参数，这就说明参数是让多个方法共享的。
    2. springmvc 用方法来接受参数
4. springMVC是基于方法的设计,而struts是基于类

## springMVC与struts2的主要区别
``区别1：``     
Struts2 的核心是基于一个Filter即StrutsPreparedAndExcuteFilter   
SpringMvc的核心是基于一个Servlet即DispatcherServlet(前端控制器)     
``区别2：``     
Struts2是基于类开发的，传递的参数是通过类的属性传递(属性驱动和模型驱动)，所以只能设计成多例prototype   
SpringMvc是基于类中的方法开发的，也就是一个url对应一个方法，传递参数是传到方法的形参上面，所以既可以是单例模式也可以是多例模式singiton    
``区别3：``     
Struts2采用的是值栈存储请求以及响应数据，OGNL存取数据   
SpringMvc采用request来解析请求内容，然后由其内部的getParameter给方法中形参赋值，再把后台处理过的数据通过ModelAndView对象存储，Model存储数据，View存储返回的页面，再把对象通过request传输到页面去。

--- 

springMvc是基于方法的通过方法里的参数来接受前台传过来的值，默认使用的是单例线程不安全，它是通过servlet实现的请求转发和初始化，开发效率高于Struts2.dispatherServlet是前端控制器，springMvc中的Interceptor是通过HandlerInterceptor来实现的。                
Struts2是基于类的通过声明全局的私有属性并生成get、set方法来接收前台传过来的值，默认是使用多例线程安全，Struts2是通过filter实现的请求转发和初步处理。Struts2有自己的拦截机制，配置在Struts.xml中，拦截Struts的action请求并处理，struts2核心控制器是FilterDispatcher 旧版本，StrutsPrepareAndExecuteFilter 新版本。         



## springMVC的运行原理
整个处理过程从一个HTTP请求开始：
1. Tomcat在启动时加载解析web.xml,找到spring  mvc的前端总控制器DispatcherServlet,并且通过DispatcherServlet来加载相关的配置文件信息。
2. DispatcherServlet接收到客户端请求，找到对应HandlerMapping，根据映射规则，找到对应的处理器（Handler）。
3. 调用相应处理器中的处理方法，处理该请求后，会返回一个ModelAndView。
4. DispatcherServlet根据得到的ModelAndView中的视图对象，找到一个合适的ViewResolver（视图解析器），根据视图解析器的配置，DispatcherServlet将要显示的数据传给对应的视图，最后显示给用户。

---

1. 用户发送请求至前端控制器 DispatcherServlet    
2. DispatcherServlet 收到请求调用 HandlerMapping 处理器映射器。   
3. 处理器映射器找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成) 一并返回给 DispatcherServlet。    					 			 	 
4. DispatcherServlet 调用 HandlerAdapter 处理器适配器   
5. HandlerAdapter 经过适配调用具体的处理器(Controller，也叫后端控制器)。  
6. Controller 执行完成返回 ModelAndView   
7. HandlerAdapter 将 controller 执行结果 ModelAndView 返回给 DispatcherServlet   
8. DispatcherServlet 将 ModelAndView 传给 ViewReslover 视图解析器   
9. ViewReslover 解析后返回具体 View    
10. DispatcherServlet 根据 View 进行渲染视图（即将模型数据填充至视图中）。   
11. DispatcherServlet 响应用户  

## springMVC的工作流程和原理是什么 {id="springmvc_1"}
<img src="mvc_process_principle.jpeg" alt=""/>



## springMVC的优点 {id="springmvc_2"}
1. 它是基于组件技术的.全部的应用对象,无论控制器和视图,还是业务对象之类的都是java组件.并且和Spring提供的其他基础结构紧密集成.
2. 不依赖于Servlet API(目标虽是如此,但是在实现的时候确实是依赖于Servlet的)
3. 可以任意使用各种视图技术,而不仅仅局限于JSP
4. 支持各种请求资源的映射策略
5. 它应是易于扩展的  

## 谈谈你对springMVC框架的看法 {id="springmvc_3"}
我最近面试过程中很少有问道框架这方面的，说实话，这框架这块如果是正常使用的话还真没啥说的，springMVC就是一个控制层框架，整体使用过程中也没遇到过什么问题，在我的理解当中也就比struts2框架节省内存，在与spring的整合过程中不需要引入额外的jar包，没有出现过安全漏洞。



## SpringMVC有哪些注解 {id="springmvc_4"}

1. @Controller，在SpringMVC 中，控制器Controller 负责处理由DispatcherServlet 分发的请求
2. @RequestMapping，RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径
3. @Resource和@Autowired  ，@Resource和@Autowired都是做bean的注入时使用，其实@Resource并不是Spring的注解，
4. @requestParam，@requestParam主要用于在SpringMVC后台控制层获取参数，类似一种是request.getParameter("name")，它有三个常用参数：defaultValue = "0", required = false, value = "isApp"；defaultValue 表示设置默认值，required 铜过boolean设置是否是必须要传入的参数，value 值表示接受的传入的参数类型。
5. @ResponseBody作用： 该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区
6. @Component  相当于通用的注解，当不知道一些类归到哪个层时使用，但是不建议
7. @Repository 用于注解dao层，在daoImpl类上面注解
8. @ModelAttribute和 @SessionAttributes代表的是：该Controller的所有方法在调用前，先执行此@ModelAttribute方法，可用于注解和方法参数中，可以把这个@ModelAttribute特性，应用在BaseController当中，所有的Controller继承BaseController，即可实现在调用Controller时，先执行@ModelAttribute方法
9. @PathVariable 用于将请求URL中的模板变量映射到功能处理方法的参数上，即取出uri模板中的变量作为参数

---

1. @RequestMapping声明方法的具体访问路径，@RequestMapping放在类上时相当于struts2的@nameSpace，就是在具体访问路径之前再加一层目录。
2. @RequestParam相当于request.getAttribute()方法，从request作用域对象中获取对应的参数值。
3. @ResponseBody 把返回的参数转换成xml或者json格式，主要与ajax请求结合使用。

--- 

1. @Controller 控制器定义 ,和Struts1一样，Spring的Controller是Singleton的。这就意味着会被多个请求线程共享。因此，我们将控制器设计成无状态类。
2. @RequestMapping 在类前面定义，则将url和类绑定。在方法前面定义，则将url和类的方法绑定
3. @RequestParam 一般用于将指定的请求参数付给方法中形参
4. @SessionAttributes 将ModelMap中指定的属性放到session中
5. @ModelAttribute 这个注解可以跟@SessionAttributes配合在一起用。可以将ModelMap中属性的值通过该注解自动赋给指定变量。
6. @ResponseBody 该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。
7. @Autowired 注解是按照类型（byType）装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许null值，可以设置它的required属性为false。如果我们想使用按照名称（byName）来装配，可以结合@Qualifier注解一起使用
8. @Resource 默认按照ByName自动注入

---

SpringMVC的注解有很多，常用的主要有@Controller声明此类是一个控制层类，后续dispatchServlet在纷发请求时就可以请求到该类。 @RequestMapping声明方法的具体访问路径，@Controller的注解需要与 @RequestMapping结合使用才能访问到控制层的具体方法，@RequestMapping放在类上时相当于struts2的@nameSpace，就是在具体访问路径之前再加一层目录，@requestParam相当于request.getAttribute()方法，从request作用域对象中获取对应的参数值，、@PathVariable解析@RequestMapping路径中的变量参数，需要与@RequestMapping结合使用，整体使用感觉和@requestParam差别不大。、@ResponseBody 把返回的参数转换成xml或者json格式，主要与ajax请求结合使用。


## MVC设计模式/Web项目的三层架构
1. M model 模型层   
2. V view 视图层    
3. C controller 控制层  

是一种将模型层、视图层与控制层的业务进行分离的设计模式，提高了代码的可读性与重用性。分离视图层和业务逻辑层也使得WEB应用更易于维护和修改。


## springMVC为什么可以是单例的，而struts2必须是多例的 {id="springmvc-struts2_1"}
springMVC的参数都是在方法中传递的，使用的都是局部变量，该变量的生命周期仅仅存在在方法中，方法调用完毕，参数就会被销毁，所以即使springMVC是单例的，也不会影响到控制层方法的调用，struts2框架之所以必须是多例的，那是因为他使用了全局变量，通过get和set方法赋值。如果struts2框架是单例的，那么就会造成下一个请求还能看到上一个请求的参数，造成信息泄露和影响当前请求的正确执行，所以struts2框架要设置为多例的，每次请求创建一个新的对象，新旧对象之间参数互不影响，这样也就造成了struts2框架对内存的消耗大的问题。

---

Spring MVC Controller默认是单例的：单例的原因有二：
1. 为了性能，单例不用每次都new，速度效率变快。
2. 不需要多例，只要controller中不定义属性，那么单例完全是安全的。  
Struts2多例原因很简单，Struts2最常用值栈来在action与页面中传递数据，它的方式是定义对象属性，不同的请求直接对象属性需要互不干扰。   
当然，如果不使用对象属性，不使用值栈的话，可以将action设置为单例的。  


## 单例和多例的区别
1. 什么是单例、多例:   
    所谓单例就是所有的请求都用一个对象来处理，比如我们常用的service和dao层的对象通常都是单例的，而多例则指每个请求用一个新的对象来处理，比如action;
2.  单例模式和多例模式说明：  
     1）单例模式和多例模式属于对象模式。  
     2）单例模式的对象在整个系统中只有一份，多例模式可以有多个实例。  
     3）它们都不对外提供构造方法，即构造方法都为私有。
3. 为什么用单例、多例：  
    之所以用单例，是因为没必要每个请求都新建一个对象，这样子既浪费CPU又浪费内存；  
    之所以用多例，是为了防止并发问题；即一个请求改变了对象的状态，此时对象又处理另一个请求，而之前请求对对象状态的改变导致了对象对另一个请求做了错误的处理；  
    用单例和多例的标准只有一个：当对象含有可改变的状态时（更精确的说就是在实际应用中该状态会改变），则多例，否则单例；
4. 何时用单例？何时用多例？  
    对于struts2来说，action必须用多例，因为action本身含有请求参数的值，即可改变的状态；  
    而对于struts1来说，action则可用单例，因为请求参数的值是放在actionForm中，而非action中的；  
    另外要说一下，并不是说service或dao一定是单例，标准同第3点所讲的，就曾见过有的service中也包含了可改变的状态，同时执行方法也依赖该状态，但一样用的单例，这样就会出现隐藏的BUG,而并发的BUG通常很难重现和查找；  
    
> [单例和多例的区别](https://www.cnblogs.com/zdf159/p/7595382.html)  


## SpringMVC的控制器是不是单例模式，如果是有什么问题,怎么解决 {id="springmvc_5"}
是。单例如果有非静态成员变量保存状态会有线程安全问题。      
``解决办法1：``不要有成员变量，都是方法。   
``解决办法2：``@Scope(“prototype”)      


## springMVC的传参方式，批量接收的写法 {id="springmvc_6"}

```java
/**
 *第一种传值方式
 * @param id
 * @return http://localhost:8080/UserApplication/test2/100
 */
@RequestMapping(value = "/test2/{id}", method = RequestMethod.GET)
public String test2(@PathVariable("id") Integer id) {
    return "id:" + id;
}
/**
 * 第二种传值方式
 * @param id
 * @return http://localhost:8080/UserApplication/test3?id=100
 * 设置默认值 这个是传参
 * 当他为false 时，使用这个注解可以不传这个参数 true时必须传 required默认值是true
 */
@RequestMapping(value = "/test3", method = RequestMethod.GET)
public String test3(@RequestParam(value = "id", required = false, defaultValue = "0") Integer id) {
    return "id:" + id;
}
```

## springMVC 对日期的处理，和附件的操作 {id="springmvc_7"}
日期的处理：在做web开发的时候，页面传入的都是String类型，SpringMVC可以对一些基本的类型进行转换，但是对于日期类的转换可能就需要我们配

1. 如果查询类使我们自己写，那么在属性前面加上@DateTimeFormat(pattern = "yyyy-MM-dd")，即可将String转换为Date类型，如下	

```java
@DateTimeFormat(pattern = "yyyy-MM-dd")
private Date createTime;
```
1. 如果我们只负责web层的开发，就需要在controller中加入数据绑定：

```java
@InitBinder
public void initBinder(WebDataBinder binder) { 
    SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd"); 
    dateFormat.setLenient(false); 
    //true:允许输入空值，false:不能为空值
    binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true)); 
}
```

1. 可以在系统中加入一个全局类型转换器


```xml
<mvc:annotation-driven>
    <bean  id="conversionnService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <list>
                <bean class="com.doje.XXX.web.DateConverter" />
            </list>
        </property>
    </bean>
<mvc:annotation-driven conversion-service="conversionService" />
```

```java
public class DateConverter implements Converter<String, Date> { 
    @Override 
    public Date convert(String source) { 
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd"); 
        dateFormat.setLenient(false); 
        try { 
            return dateFormat.parse(source); 
        } catch (ParseException e) { 
            e.printStackTrace(); 
        } 
    return null; 
}
```
1. 如果将日期类型转换为String在页面上显示，需要配合一些前端的技巧进行处理。
2. SpringMVC使用@ResponseBody返回json时，日期格式默认显示为时间戳。
```xml
<mvc:annotation-driven> 
    <mvc:message-converters> 
        <bean class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter"> 
            <property name="objectMapper" ref="customObjectMapper"></property> 
        </bean> 
    </mvc:message-converters> 
</mvc:annotation-driven>
```

```java
@Component("customObjectMapper") 
public class CustomObjectMapper extends ObjectMapper { 
    public CustomObjectMapper() { 
        CustomSerializerFactory factory = new CustomSerializerFactory(); 
        factory.addGenericMapping(Date.class, new JsonSerializer<Date>() { 
            @Override 
            public void serialize(Date value,JsonGenerator jsonGenerator,SerializerProvider provider) throws IOException, JsonProcessingException { 
                SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"); 
                jsonGenerator.writeString(sdf.format(value)); 
            } 
        }); 
        this.setSerializerFactory(factory); 
    } 
}
```

1. date类型转换为json字符串时，返回的是long time值，如果需要返回指定的日期的类型的get方法上写上@JsonFormat(pattern="yyyy-MM-dd HH:mm:ss",timezone = "GMT+8") ，即可将json返回的对象为指定的类型。

```java
@DateTimeFormat(pattern="yyyy-MM-dd HH:mm:ss") 
@JsonFormat(pattern="yyyy-MM-dd HH:mm:ss",timezone = "GMT+8") 
public Date getCreateTime() { 
    return this.createTime; 
}
```

