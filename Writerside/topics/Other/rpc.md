# rpc

## webService和httpclient区别。
httpclient对认证机制+、提供了全面的支持。用户认证过程需要一组用户名/密码进行认证用户身份。HttpClient附带多个AuthScheme的实现。  AuthScheme接口表示一个抽象的面向挑战-应答的认证机制。解析和处理目标服务器发送的挑战并且对受保护资源的请求做出应答。        

webService一般通过iis进行匿名调用。这种方式不安全。我们可以使用CXF的拦截器通过自定义的Interceptor，可以改变请求和响应的一些消息处理，其中最基本的原理还是一个动态代理。设置拦截器。来验证用户的账号和密码。     

webService是完全基于XML的，HttpClient是基于HTTP协议的；WebService传输的是对象，HttpClient传输的是JSON格式的数据；

## webservice和项目怎么整合？
首先1.我们当时发布webservice接口用的是cxf技术，当时用的是maven项目，然后呢在web.xml里配置cxfServlet,里面有一个url-parent指明cxf的请求地址,我们在pom.xml中导入cxf的相关jar包，然后我们在spring-cxf.xml,在这个文件里面首先通过import引入cxf的配置文件，有三个（那三个）然后在`<jaxws:endpoint(en de po int)>`里配置一个id不能重复，写一个implementor跟着接口的地址，address代表-----接口的访问地址，在webservice真正发布的实现类的接口里加上@webservice注解，接口的方法上加上@webmethod注解，在webMethod方法中有参数，请求参数加上@param注解。使用EndPoint.publish方法发布服务，这样一个webservice的接口就配置好了。        

启动项目，访问发布的地址，在浏览器输入端口号+cxfServlet里面配置的url-parent的地址，spring-cxf.xml里面写接口的地址？wsdl回车，看一下wsdl文档是否完整，如果浏览器响应出来刚才指明方法的名字则webservice接口发布成功。

## 如何保证cxf接口的安全性？
首先webservice还是使用cxf，cxf里面自身支持一个拦截器，我们发布的接口里面加一个自定义的Interceptor，在里面配置一个PasswordCallback，密码认证，然后在密码认证的类里写明实现一个接口叫CallbackHandler ，在里面指明需要认证的用户名和密码，也就是说从里面把用户名和密码拿过来放到cxf里面，然后cxf就会帮我们自动去完成用户名和密码认证，如果认证通过则会调到接口，如果认证不通过则会抛出异常。


## httpclient如何保证接口安全？
1. 它自身没有CXF机制，所以得做安全认证，首先有的httpclient入口加上一个统一的签名认证，也就是说签名认证通过之后，才能调用详细的业务参数处理。怎么签名认证呢？根据一定的算法，根据传过来的参数，以及参数排序，排完之后进行MD5加密，加密之后把签名拿过来，我们把请求方请求的签名同样做一遍和我们签名认证相同的加密和签名认证作比较，如果它的加密之后和我加密之后的一样，证明签名认证通过，否则签名认证不通过。
2. 保证我们接口与接口之间在内网相互调用
3. v*n：尽管在同一个局域网内，但是必须通过V*N进行认证，两台服务器通过专用通道V*N，别人访问不到保证httpclient接口的安全。

## webservice cxf
`服务端：`      
需要导入cxf-rt-transports-http-jetty.jar包，写一个接口和实现类， 在接口的类名上需加@WebService注解，接口的方法上需加@WebMethod注解， 之后写一个发布类，使用EndPoint.publish()方法发布服务，访问发布的地 址，查看生成的wsdl文档是否完整（完整的wsdl文档包含types  messages portType  binding service五部分）；

`客户端：`
首先根据服务端发布的地址使用cxf框架工具或jdk（1.6版本以上）生成中间桥梁类，然后写一个中间桥梁类的服务类，通过调用中间桥梁类中的方 法来访问服务端数据，从而达到了远程调用的目的Spring与cxf结合   

`webservice服务端配置流程`     

首先在web.xml中引入cxfServlet核心类，指定对以/cxf开头的url路径 提供webservice服务，之后我们在要发布成webservice接口上添加 @Webservice 注解，而且还要在实现类上添加同样的webservice注解并且 要说明实现了哪个接口，之后在spring-webservice.xml中发布webservice 服务，通过jaxws:endpoint这个标签，并且在标签配置implementor和 address来表明实现服务的类，以及发布的地址，最后在浏览器中输入相关的webservice地址?wsdl来验证服务是否发布成功。        
    
`webservice客户端的配置`      
首先通过wsdl2java根据发布的webservice服务端地址的wsdl生成客户端 调用的中间桥梁java类，将生成的java类拷贝到客户端项目中，配置spring-client.xml文件，通过jaxws:client定义一个bean,并通过address 属性指明要访问的webservice的服务地址，通过serviceClass指明充当中 间桥梁的服务类，之后获取该bean,就可以通过它来访问发布的webservice接口中的方法。
