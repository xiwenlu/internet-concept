# spring MVC

## 什么是 MVC?
MVC（Model-View-Controller）是一种软件设计模式，用于构建用户界面，特别是在Web应用中。它将应用分为三个组件：
1. Model：包含业务逻辑和数据，处理数据的存储和操作。
2. View：显示用户界面，从Model获取数据并呈现给用户。
3. Controller：协调Model和View，处理用户输入，更新Model并刷新View。

这种模式的好处是将数据处理、界面展示和用户交互分开，提高代码的可维护性、可测试性和灵活性。许多Web框架，如Spring MVC、Rails等，都是基于MVC模式设计的。

## 什么是 Spring MVC？

Spring MVC 是 Spring 框架的一个模块，用于构建基于 Java 的 Web 应用程序。它是一个模型-视图-控制器（MVC）架构的实现，遵循分层设计原则，将应用程序的业务逻辑、数据和用户界面（UI）组件分离，以提高代码的可测试性和可维护性。

Spring MVC 提供了以下核心功能：
1. DispatcherServlet：作为前端控制器，它负责接收 HTTP 请求，根据请求信息（如 URL 和请求方法）调度到相应的处理器（Controller）。
2. Model：模型部分通常由业务对象和数据访问对象组成，负责处理业务逻辑和数据操作。
3. Controller：控制器处理请求，调用模型进行业务处理，并决定如何响应。它将模型数据绑定到视图中。
4. View Resolver：视图解析器根据控制器返回的逻辑视图名称，查找并渲染实际的视图（如 JSP、Thymeleaf、Freemarker 等）。
5. Data Binding：Spring MVC 支持将请求参数自动绑定到控制器方法的参数上，以及将模型数据绑定到视图中。
6. Validation：提供了验证机制，可以验证用户输入的有效性。
7. Internationalization (i18n)：支持多语言，可以根据用户首选的语言提供本地化内容。
8. Exception Handling：可以自定义异常处理策略，将异常转换为有意义的响应。
9. RESTful Support：支持创建 RESTful 风格的 Web 服务。

Spring MVC 可以与其他 Spring 模块（如 Spring Core、Spring Data、Spring Security 等）无缝集成，提供全面的 Web 应用开发解决方案。由于其灵活性和强大的功能，Spring MVC 成为了许多企业级 Java Web 应用的首选框架。


## Spring MVC的优点？ {id="advantage"}

Spring MVC作为Spring框架的一部分，具有以下显著优点：
1. 松耦合：通过依赖注入（DI）和面向接口编程，降低了组件间的耦合，增强了代码的可测试性和可维护性。
2. 注解驱动：大量使用注解减少XML配置，使得开发更加简洁和快速。如@Controller、@RequestMapping、@Autowired等。
3. 模型-视图-控制器架构：清晰的MVC设计模式使得职责分离明确，便于团队协作和代码管理。
4. 数据绑定和验证：内置的数据绑定和验证机制简化了模型数据与请求参数的映射和验证过程。
5. 国际化支持：提供i18n（国际化）支持，方便创建多语言应用。
6. 异常处理：自定义的异常处理机制，可以优雅地处理错误和异常。
7. RESTful支持：支持创建符合REST原则的API，适应现代Web服务的需求。
8. 视图技术的灵活性：支持多种视图技术，如JSP、Thymeleaf、FreeMarker等，可根据项目需求选择。
9. 可扩展性：通过拦截器（Interceptor）、自定义注解等机制，可以方便地扩展和定制功能。
10. 性能：经过优化的DispatcherServlet和视图解析器，能够高效处理请求。
11. 强大的社区和生态系统：Spring有一个庞大的开发者社区，丰富的第三方库和插件，以及详尽的文档和教程。
12. 与Spring框架的集成：可以无缝集成Spring的其他模块，如Spring Security、Spring Data等，构建完整的应用解决方案。
13. 测试友好：支持单元测试和集成测试，方便编写测试代码。

这些优点使得Spring MVC成为开发企业级Web应用的流行选择。


## Spring MVC的缺点？ {id="shortcoming"}
尽管Spring MVC是一个强大且广泛使用的Web框架，但它也有一些潜在的缺点和挑战，包括：
1. 学习曲线：对于初学者，尤其是那些不熟悉Spring框架的人来说，Spring MVC的学习曲线可能会比较陡峭。它涉及到许多概念，如依赖注入、AOP、MVC模式等，需要时间去理解和掌握。
2. 配置复杂性：虽然现代Spring MVC通过注解大大减少了XML配置，但在复杂的项目中，配置管理仍可能变得复杂，特别是当需要精细控制框架行为时。
3. 过度设计：对于小型项目，Spring MVC的灵活性和功能丰富性可能显得有些过度，导致项目结构过于复杂，增加了不必要的开销。
4. 启动和运行时资源消耗：Spring应用程序，特别是配置较多时，启动时间可能较长，内存占用也可能较大。
5. 视图与控制器的紧密耦合：尽管鼓励分离，但在实践中，视图和控制器之间的关系有时可能过于紧密，尤其是在没有适当设计的情况下。
6. 过度依赖框架：高度依赖Spring框架的特性可能导致代码在没有Spring环境时难以重用或测试。
7. 性能考虑：虽然Spring MVC本身是高效的，但在处理大量并发请求时，如果没有恰当的配置和优化，性能可能会受到影响。
8. 版本兼容性：随着Spring版本的更新，有时需要迁移旧代码，这可能涉及到不兼容的更改和升级成本。
9. 调试和异常处理：在复杂的应用场景下，异常处理和调试可能需要更多技巧，特别是当涉及到Spring的自动配置和AOP时。
10. 社区资源的多样性：尽管Spring社区庞大，但对于特定问题的解决方案可能分散，新手可能需要时间找到最适合的解答。

了解这些缺点有助于在项目选型时做出更合适的选择，并在使用Spring MVC时采取措施来克服这些挑战。

## Spring MVC的控制器是不是单例模式 {id="Single_model"}

Spring MVC的控制器默认是单例模式。这意味着在整个应用程序的生命周期中，Spring容器只会创建每个控制器类的一个实例。这种设计决策主要是出于性能考虑，因为它减少了实例化控制器对象的开销，尤其是在高并发环境下，可以更高效地复用同一个控制器实例处理多个请求。

然而，单例模式在控制器中引入了线程安全和状态管理的挑战。如果控制器中存在非静态成员变量并且这些变量的状态在请求间需要改变，就可能导致线程安全问题，因为多个线程可能同时访问这些共享状态。为了解决这些问题，开发者通常会避免在控制器中保持请求之外的业务状态，或者使用线程安全的类来管理状态，或者在必要时通过Spring的scope配置将控制器设置为原型模式（@Scope("prototype")），但这通常不是推荐的做法，因为会增加资源消耗和管理复杂性。

## Spring MVC是 Spring 框架的关系
Spring MVC 是 Spring 框架的一个子模块，专注于构建Web应用的MVC部分。它与Spring核心框架紧密结合，利用Spring的依赖注入等特性来管理MVC组件。Spring提供了基础的IoC和AOP等功能，而Spring MVC扩展了这些能力，用于处理HTTP请求，实现请求-响应的生命周期管理，包括路由控制、模型数据处理、视图渲染等。在实际开发中，Spring MVC依赖于Spring容器来管理控制器和其他Web组件，形成一个完整的Web应用开发解决方案。简单来说，Spring是全面的应用框架，Spring MVC是其中专门针对Web应用开发的模块。


## Spring MVC的运行流程 {id="spring-mvc_1"}

Spring MVC 的运行流程大致如下：
1. 请求开始：用户通过HTTP请求访问应用，请求包含URL和参数。
2. DispatcherServlet：请求首先到达Spring MVC的前端控制器DispatcherServlet，它负责调度请求。
3. 请求映射：DispatcherServlet使用HandlerMapping找到匹配的控制器方法，基于@RequestMapping等注解。
4. 处理器适配器：DispatcherServlet通过HandlerAdapter调用控制器方法，适配器知道如何处理不同类型的控制器。
5. 执行业务逻辑：控制器方法执行，处理请求，可能包括数据处理、服务调用等。
6. 模型和视图：控制器返回ModelAndView或直接返回视图名称，包含模型数据和视图信息。
7. 视图解析：ViewResolver根据视图名称解析出具体的视图实现，如JSP、Thymeleaf等。
8. 渲染视图：视图使用模型数据生成HTML响应。
9. 响应用户：DispatcherServlet将HTML响应发送回客户端，结束请求。

在整个过程中，Spring MVC还支持拦截器和异常处理，提供更灵活的控制和错误处理。

## Spring MVC中的核心接口 {id="interface"}
Spring MVC 中的核心接口主要包括：
1. DispatcherServlet：作为前端控制器，负责接收请求，调度请求处理流程。
2. HandlerMapping：映射请求到相应的处理器（Controller 方法）。
3. Controller：通常通过@Controller注解标记的类，处理请求并返回响应。
4. HandlerAdapter：适配器接口，调用控制器方法并处理其返回结果。
5. ViewResolver：解析逻辑视图名到实际视图对象，如JSP或Thymeleaf模板。
6. HandlerInterceptor：拦截器接口，允许在请求处理前后执行额外逻辑，如认证、日志记录等。
7. ModelAndView：封装模型数据和视图信息，用于返回给DispatcherServlet。

这些组件协同工作，形成了一条从请求接收、业务处理到响应生成的完整处理链路，简化了Web应用的开发和维护。

## SpringMVC有哪些注解？ {id="annotation"}

Spring MVC 提供了一系列注解，用于简化控制器、请求映射、数据绑定和其他功能的配置。以下是一些核心的注解：
1. @Controller: 用于标记一个类作为Spring MVC的控制器，表明该类将处理HTTP请求。
2. @RequestMapping: 用于将HTTP请求映射到控制器方法，可以注解在类级别或方法级别，指定URL模式。
3. @GetMapping / @PostMapping / @PutMapping / @DeleteMapping: 这些是@RequestMapping的特殊形式，分别对应HTTP的GET、POST、PUT和DELETE请求。
4. @RequestParam: 将请求参数绑定到方法参数，例如@RequestParam("paramName") String paramName。
5. @PathVariable: 用于获取URL模板变量的值，如@PathVariable("id") Long id。
6. @RequestBody: 用于将HTTP请求体中的JSON或XML数据转换为Java对象。
7. @ResponseBody: 表示方法的返回值将直接写入HTTP响应体，通常用于处理JSON或XML响应。
8. @ModelAttribute: 用于从请求中获取数据并将其绑定到模型对象，或者将模型对象添加到模型视图中。
9. @ExceptionHandler: 注解在方法上，用于捕获和处理特定的异常。
10. @InitBinder: 可以用于初始化数据绑定的行为，例如设置日期格式或排除某些字段。
11. @Autowired / @Qualifier: Spring的依赖注入注解，用于自动装配依赖的bean，@Qualifier用于指定具体哪个bean。
12. @ControllerAdvice: 提供全局异常处理和数据绑定建议，可以作用于整个应用的控制器。

## @RequestBody注解

POST请求通常使用@RequestBody来接收请求体中的数据，尤其是在发送JSON、XML等非表单格式的数据时。这是因为在RESTful设计原则中，POST请求常用于创建资源，而资源的具体内容通常位于请求体中。使用@RequestBody可以将请求体的内容直接映射到方法的参数上，便于处理复杂的数据结构。

@RequestBody注解确实通常用于接收JSON对象或数组，而不是简单的基础类型，如String。在您的示例中，您尝试将@RequestBody应用于List&lt;String&gt;，这在大多数情况下是可行的，前提是在请求体中，前端发送的是一个JSON数组，例如：

```json
[
 "id1",
 "id2",
 "id3"
]
```

这些注解极大地简化了Spring MVC应用的配置，让开发者可以更专注于业务逻辑而不是底层的HTTP处理细节。

## Spring MVC如何处理日期类型？ {id="date_type"}
Spring MVC在处理日期类型时，通过注解、全局配置或自定义转换器实现灵活的转换机制。你可以使用@DateTimeFormat直接在参数上指定格式，实现局部日期时间格式化。对于全局设置，可以通过配置文件指定默认日期格式。此外，通过实现Formatter或Converter接口，可以定制化日期转换逻辑，适应复杂需求。这些方式确保了日期数据在HTTP请求与后端处理间的准确无误转换。

## spring MVC的传参方式 {id="param_type"}
Spring MVC支持多种参数传递方式，包括：
1. 基本类型和String参数：直接声明在方法参数中，Spring MVC会自动绑定请求参数。
2. @RequestParam：用于指定请求参数名和是否必需，支持默认值。
3. 对象绑定：通过Java Bean类映射请求参数，自动填充对象属性。
4. @ModelAttribute：用于模型数据的绑定和存储，适用于多参数或预加载数据。
5. Path Variables：从URL模板中提取变量，如/user/{userId}。
6. 数组和集合：通过[]接收数组或List类型参数。
7. HttpServletRequest和HttpSession：直接操作请求或会话对象获取所有参数。
8. JSON数据绑定：使用@RequestBody接收JSON格式的请求体，自动转换为对象。

这些方式覆盖了各种场景，帮助开发者灵活处理HTTP请求中的参数。


## Spring MVC、Struts 1 和 Struts 2的区别
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


