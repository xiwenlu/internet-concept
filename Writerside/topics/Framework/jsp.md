# jsp


## JSP的本质是什么？ {id="jsp_1"}
1. JSP页面在本质上就是Servlet程序，当JSP页面在首次被访问时，Ｗeb容器就会将JSP页面转化为Servlet，只需要做一次。如index.jsp在首次被访问时，Ｗeb容器会将其翻译成一个index_jsp.java文件，即Servlet代码；
2. Servlet程序要被WEB容器调用执行，必须在web.xml中注册映射；
3. Servlet类继承了类org.apache.jasper.runtime.HttpJspBase(Tomcat API,Tomcat容器实现的),这个类实现了HttpJspPage, JspPage, Servlet, ServletConfig等这些接口，因此WEB容器必须实现这些接口。

## jsp中EL表达式的语法
Expression Language ${expression}


## jsp的运行原理 {id="jsp_2"}
1. 用户发送一个请求---->服务器(tomcat)
2. 服务器根据请求的url(地址)----->找到相应的jsp文件
3. 将找到的jsp文件转译成java源文件(servlet)
4. 将转译出来的java源文件进行编译，得到一个可执行文件(.class)
5. 执行.class文件并且告诉服务器(tomcat)
6. 服务器返回一个响应信息------>用户

## 九大隐式对象
输入/输出对象：  	request   	response   	out     
作用域通信对象：	session  	application  	pageContext     
servlet 对象：   	page   	config      
错误对象：      	exception    
四个作用域从大到小：application > session > request > page      

## jsp的九大隐式对象 {id="jsp_3"}
输入输出对象       
1. request      
对应的实体类是(HttpServletRequest)，获取请求的信息(这是用户提交上来的信息)，同时还是作用域通讯对象，在当前请求中，一直有效。         
2. response	    
对应的实体类是(HttpServletResponse)，响应用户的信息(这是要返回给用户的信息)。            
3. out	        
对应的实体类是(JspWriter)；输出对象，一般输出一些调试信息。
```jsp
out.println();			// 向页面输出一些信息
System.out.println();	// 向控制台输出一些信息
```
---
作用域通讯对象
1. application	            
对应的实体类是(ServletContext)，只要服务器不关闭，一直有效。           
2. session	            
对应的实体类是(HttpSession)，只要当前会话不关，一直有效。           
3. pageContext	        
对应的实体类是(PageContext)，只在本页面上有用(离开里本页面就没用了)，request在当前请求中，一直有效。
   
---
servlet对象
1. page		        
对应的实体类是(Page)，相当于new了一个实例，类似于java中的this关键字。            
2. config	            
对应的实体类是(ServletConfig)，可以获取servlet初始化的一些信息，不太常用。        
   
---
错误对象      
exception   对应的实体类是(Throwable)，专门处理错误。	        

---
注意：所有的九大隐式对象都拥有这几个常用方法
```jsp
setAttribute(key, value);	// 设置属性 	
getAttribute(key);          // 获取属性值	
removeAttribute(key);       // 移除某个属性	
```

特点：
1. 不需要实例化(new)，直接拿过来就能用。
2. 如果想要使用exception对象，必须在jsp指令中设置`<%@ page isErrorPage="true"%>`。


## session 和 cookie 的区别
session 是存储在服务器端，cookie 是存储在客户端的，所以安全来讲 session 的安全性要比 cookie 高，然后我们获取 session 里的信息是通过存放在会话 cookie 里的 sessionid 获取的。

又由于session是存放在服务器的内存中，所以session里的东西不断增加会造成服务器的负担，所以会把很重要的信息存储在 session 中，而把一些次要东西存储在客户端的cookie里，然后cookie确切的说分为两大类分为会话cookie和持久化cookie，会话cookie确切的说是存放在客户端浏览器的内存中，所以说他的生命周期和浏览器是一致的，浏览器关了会话cookie也就消失了，然而持久化cookie是存放在客户端硬盘中，而持久化cookie的生命周期就是我们在设置cookie时候设置的那个保存时间，然后我们考虑一问题当浏览器关闭时session会不会丢失，从上面叙述分析session的信息是通过sessionid获取的，而sessionid是存放在会话cookie当中的，当浏览器关闭的时候会话cookie消失所以我们的sessionid也就消失了，但是session的信息还存在服务器端，这时我们只是查不到所谓的session但它并不是不存在。

那么，session在什么情况下丢失，就是在服务器关闭的时候，或者是session过期，再或者调用了invalidate()的或者是我们想要session中的某一条数据消失调用session.removeAttribute()方法，然后session在什么时候被创建呢，确切的说是通过调用session.getsession来创建，这就是session与cookie的区别   

## servlet生命周期？servlet在什么时候实例化？在什么时候初始化？doget和dopost有什么区别？在什么时候销毁？
实例化、初始化、提供服务、销毁、回收      
如果一个servlet的实例并不存在，Web容器加载servlet类。创建一个servlet类的实例。只有在第一次访问的时候调用init初始化servlet实例。Service调用doget()、dopost()提供服务，servlet容器关闭是调用destroy销毁来结束该servlet。    
doget和dopost其实并没有什么本质的区别,当form框里面的method为get时，执行doGet方法，使用get提交就必须在服务器端用doGet()方法接收；当form框里面的method为post时，执行doPost方法，使用post提交就必须在服务器端用doPost()方法接收


## servlet的生命周期
1. 实例化：由web服务器进行servlet的实例化
2. 初始化：使用init()方法进行初始化
3. 提供服务：使用service()方法提供服务。（具体使用doGet()和doPost()方法提供服务）
4. 销毁：使用destroy()方法进行销毁
5. 不可用：标记为垃圾回收



## forword(请求转发)与redirect(重定向)
1.从数据共享上          
Forword是一个请求的延续，可以共享request的数据。Redirect开启一个新的请求，不可以共享request的数据。              
2、从地址栏     
Forword转发地址栏不发生变化。Redirect转发地址栏发生变化。   


## 转发和重定向的区别
redirect重定向     
特点：地址栏会发生变化，数据不共享(用户请求信息不会带到新页面)。执行效率低，好几次请求。       
写法：`response.sendRedirect(目标文件地址);`
		
forward转发       
特点：地址栏不会发生变化，数据共享(用户请求信息会被带到新页面)。执行效率高，只有一次请求。      
写法：`request.getRequestDispatcher(目标文件地址).forward(request, response);`


## jsp页面的七个元素以及定义方法
静态内容：css、js。

指令：以"<%@" 开始，以"%>"结束。
jsp指令影响由jsp页面生成的servlet的整体结构。       
jsp中的指令主要有3种：page、include、taglib。       
1. page可以放置在任意的位置，通常我们通过类的导入，servlet超类的定制，内容类型的设定以及诸如此类的事务来控制servlet的结构。
2. include(只能导入页面)的使用   
3. taglib 定义自定义标签的使用（声明本页面的自定义标签）   比如：c标签的声明   

表达式：  <%=java代码%>   java代码包括运算，调用方法等等。   注意：表达式中不可以加分号，表达式中的内容默认打印到页面。      

声明：    <%!变量或方法%>           

脚本：（scriptLet）  <%java代码%>     注意：脚本的内容默认不在页面展示，如果展示，需要使用页面打印方法，脚本中每行的结尾必须写分号。  

动作：   以"<jsp\:动作名>"开始，以"</jsp:动作名>"结束。       

注释：   <!-- -->   这种注释客户端可以看到。	 <%-- --%>  这种注释是用于开发人员内部，关于代码用途、逻辑等的注释。        


## include 的指令和动作
include指令的语法格式如下        
*<%@ include file="Relative Url"%>* 

jsp:include动作的完整语法如下        
*<jsp:include page="Relative path to resource" flush="true">* 

## include指令和jsp动作的区别
jsp:include动作和include指令之间的根本性的不同在于它们被调用的时间。         
jsp:include动作在请求期间被激活，include指令在页面转换前被激活。 
两者之间的差异决定着它们在使用上的区别。        
include指令的页面要比使用jsp:include动作的页面难于维护。           
include指令的功能更强大，执行速度也稍快。        
include指令:两个页面 共同产生一个 .Java  一个.class           
include动作:两个页面 分别产生一个 .Java  一个.class       
