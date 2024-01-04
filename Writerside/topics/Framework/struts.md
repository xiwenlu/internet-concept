# struts


## struts概述
1. struts2配置文件是(struts.xml),此配置文件被核心控制器默认加载时名字必须为struts.xml，必须在src路径下
2. struts2核心控制器是(FilterDispatcher 旧版本，StrutsPrepareAndExecuteFilter 新版本)
3. struts2配置文件中Action标签的class属性值是(action类的全名，也就是路径)，name属性的值是(代表Action的名称 从前台访问时的路径组成部分)
4. struts2配置文件中result标签的属性 name 的属性值与 action 类方法返回的字符串匹配, type 的属性值默认是(dispatcher 跳转页面)重定向的值是(redirect)

## struts2简述 {id="struts2_1"}
1. struts2 是一个MVC框架基于WebWork 发展而来，属于控制层框架。
2. 负责jsp页面和java代码之间的值的传递和跳转。
3. 核心控制器是FilterDispatcher 2.1.2 版本前含2.12，从2.1.3版本开始用 StrutsPrepareAndExecuteFilter代替了。

## struts2框架的搭建步骤 {id="struts2_2"}
1. 导入jar包到lib目录
2. 在web.xml内增加struts核心控制器配置
3. 将struts.xml放在src目录下
4. 新建Action类需要继承ActionSupport，方法修饰符必须是public返回值类型为String没有参数
5. 配置struts.xml中的action标签以及result标签 
6. 在页面通过struts.xml中配置的Action名称对Action进行访问

##  struts2的运行原理 {id="struts2_3"}
1. tomcat启动加载 Web.xml ，核心控制器 FilterDispatcher（新版：StrutsPrepareAndExecuteFilter） 加载并解析 struts.xml
2. 客户端发出请求到 Action ，FilterDispatcher 根据后缀名拦截
3. FilterDispatcher  根据 struts.xml 配置文件信息找到指定的Action方法
4. 执行相关的业务逻辑后 返回字符串
5. 根据返回字符串，在 struts.xml 的 \<result> 的name属性的值，进行匹配跳转到指定的jsp或重定向到其他 Action

## Struts2标签 {id="struts2_4"}
注意：一定不要忘记导入标签库`<%@ taglib prefix="s" uri="/struts-tags" %>`，取值使用 # 例：#page 与 c 标签的${page}相同
1. 迭代标签(<s:iterator>)
2. 判断标签(<s:if>)
3. 取值标签(<s:property>)
4. 格式化日期标签(<s:date name="loginName" format="yyyy-MM-dd">)
5. 输出标签(<s:property>)
6. 赋值标签(<s:set>)


## 精确匹配和模糊匹配的区别
1. 精确匹配         
struts.xml中 Action 配置 method 属性的值是 Action 类中的某一个方法，前台访问时 Action名称.action               
缺点: 配置繁琐 action 类中的每一个方法 都需要配置一个 \<Action> 标签            
优点: 分工明确 一个配置发生错误 不会影响其他Action的运行           
2. 模糊匹配         
struts.xml中，Action配置name值时通过通配符*配置请求的格式，class 和 method通过 {数字} 的方式引用 name中通配符信息，前台访问时，按Action中定义的规则.action 去访问              
缺点: 如果配置错误，所有的 Action 都不能使用             
优点: 配置文件中，需要配置的 \<action> 会变少           

## Struts2自定义拦截器的三种实现方式 {id="struts2_5"}
1. 实现接口 Interceptor
2. 继承抽象类 AbstractInterceptor 重写intercept方法（一般使用此种实现方式）
3. 继承 MethodFilterInterceptor 类，允许拦截器通过执行的方法： invocation.invoke();

## Struts2中action中如何获得request
HttpServletRequest request = ServletActionContext.getRequest();


## action类的作用
1. url：action负责url的调换      
2. 传值：action负责获取前台的值，以及将值传到前台页面（${javabeanName.javabean属性}）           
3. 属性驱动：       
    - 需要私有化属性
    - 生成get set方法
    - 给实体类赋值，需要级联操作赋值（javabeanName.javabean 属性）        
4. 模型驱动：
    - 首先需要实现 ModelDriven\<Hero> ，需要将定义泛型为Javabean（实体类）
    - 需要重写 getModel 方法 此方法返回值为 泛型 实体类
    - 私有化并实例化你的实体类

## Action类与Servlet的区别
1. Action是多实例的, 每次访问都会创建一个新的Action对象, 当返回结果调用return时action被销毁; 而Servlet是单实例的, 默认只在第一次访问时创建
2. Servlet中, 成员变量存在线程安全的问题

## 填空题
1. Struts2中action要继承哪个类(ActionSupport)

## struts2中获取参数
```java
ServletActionContext.getRequest().getSession().setAttribute("XX","XX");
```
