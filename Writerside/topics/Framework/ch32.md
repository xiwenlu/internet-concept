# dubbo

## dubbo+zookeeper 详细介绍，项目如何搭建的，如何配置的，怎么和项目整合。
Duboo是一个分布式框架，zookeeper是duboo生产者暴露服务的注册中心。起一个调度和协调功能，当然注册中心也可以用redis 或者duboo自带的Multicast(mo ti ka si te) 来注册。       

Duboo 通信方式采用长链接方式，所以当spring启动后链接就接通，duboo的消费者和生产者就可以直接调用。性能上高于其他http协议的请求。当时是为了解决单个服务器站点的压力，将项目拆分成页面加controller属于消费者，service+dao属于生产者，所有生产者暴露的端口都注册在zookeeper里面。这时候，消费者要调用生产者去zookeeper中取就可以了。所以我们部署了多套生产者，所有的消费者的请求可以由多个生产者去提供，具体由哪个生产者提供可以由zookeeper的配置去决定。如果某个生产者挂掉，zookeeper会加压力导向其他生产者，当这个生产者恢复状态的时候。Zookeeper会重新启用它。因为我们用n的执行,如果想让单个tomcat执行的action---service—dao 请求又多个tomcat来执行就可以使用 zookeeper+duboo  这时候一般是一个tomcat里面部署的是jsp+action所有的service接口都注册到了zookeeper里面，action去zookeeper里面通过duboo去调用哪个 service+dao 的组合。而且service+dao的组合可以配置多套。一套挂了其他的service+dao组合可以继续使用            

dubbo框架的体系结构有5个核心组成部分，分别是提供者provider(普发一的),它的作用是为消费者提供数据。注册中心registry(ruai几丝追),它的作用是用来注册和发现服务。消费者consumer(肯酥么儿),它的作用是调用远程提供者提供的服务。监控中心Monitor(嘛呢de er)用来统计服务的调用次数以及调用时间，还有container(肯提呢er)用来充当容器来加载，运行服务提供者。            

新建dubbo-provider(普发一的).xml配置文件，通过dubbo:application配置提供者应用名，通过dubbo:registry(ruai几丝追)配置注册中心的地址，通过dubbo:protocol(pro  to  ka)配置协议，以及通过dubbo:service来暴露要发布的接口。         

最后我们在需要使用dubbo接口的项目中配置消费者信息，新建dubbo-consumer(肯酥么儿).xml文件，通过dubbo:application配置消费者应用名,通过dubbo:registry(ruai几丝追)指明要订阅的注册中心地址，通过dubbo:reference(ruai fu 让 ci)指定要订阅的服务接口。           

配置dubbo集群来提高健壮性以及可用性。dubbo默认的集群容错机制是Failover(飞漏发)即失败自动切换，默认的重试次数为2，可以通过retries(为 chua aisi)调整。dubbo默认的负载均衡策略是Random随机，可以按权重设置随机概率。我们在写完dubbo提供者之后，为了测试接口的正确性，我们会进行直连测试。首先会在提供者端，通过将dubbo:registry(ruai几丝追)的register(ruai ji si te )设置为false,使其只订阅服务而不注册现在正在开发的服务；在消费者端，通过设置dubbo:reference(ruai fu 让 ci)的url，直连提供者进行测试。         

被动说：所谓dubbo集群就是将dubbo的提供者部署多份，在不同的机器上或者说在同一台机器上用不同的端口号。从而在启动时可以向注册中心进行注册，这样结合dubbo的集群容错策略以及负载均衡策略可以提高可用性。       

dubbo负载均衡策略：随机,轮询，最少活跃调用数。          
dubbo的集群容错:失败自动切换,快速失败,失败安全。            
dubbo+zookeerper实现session共享（在消费端）：          
我们的消费者只有一个模块，所有的请求首先都是进入这个模块里面, session都在消费者这个action里面，所有的请求都是首先进入这个项目里面，我们的生产者有多个模块。如果有多个消费者的情况下，会存在session共享问题，我们可以将session的id作为key值，用户对象作为value值存储到redis里面。当每次发送的请求的时候，拿着浏览器的session的id去redis里面取，如果能取到，证明用户已经存在，如果不能取到，就重新登录。        

## dubbo六种容错策略 {id="dubbo_1"}
1. Failover Cluster 模式      
失败自动切换，当出现失败，重试其它服务器。(缺省) 通常用于读操作，但重试会带来更长延迟。 可通过retries=”2”来设置重试次数(不含第一次)。
2. Failfast Cluster     
快速失败，只发起一次调用，失败立即报错。通常用于非幂等性的写操作，比如新增记录。
3. Failsafe Cluster     
失败安全，出现异常时，直接忽略。通常用于写入审计日志等操作。
4. Failback Cluster     
失败自动恢复，后台记录失败请求，定时重发。通常用于消息通知操作。
5. Forking(fo gen) Cluster        
并行调用多个服务器，只要一个成功即返回。通常用于实时性要求较高的读操作，但需要浪费更多服务资源。 可通过forks=”2”来设置最大并行数。
6. Broadcast(bu ruo ka si te) Cluster       
广播调用所有提供者，逐个调用，任意一台报错则报错。(2.1.0开始支持)通常用于通知所有提供者更新缓存或日志等本地资源信息  


## Dubbo优化
开发时的三个优化：       
1. 开发者在本地开发的时候启动Dubbo比较麻烦，所以采用直接连接的配置；
2. 开发者本地开发时会打断点调试，会超过Dubbo默认的超时时间1s，所以需要全局设置超时时间；
3. 开发者本地时可能会先启动消费方服务，再启动提供方服务，为了先后启动没有顺序问题，所以需要设置不检查注册中心及提供方服务；
   

1. 直接连接，即可以停止zookeeper服务；       

（1）提供方的配置：
```xml
<!-- 配置注册中心 -->
<!--     <dubbo:registry address="192.168.1.110:2181" protocol="zookeeper"/> -->
<dubbo:registry address="N/A"/>
```

（2）消费方配置：         
```xml
<!-- 注册中心 -->
<dubbo:registry address="N/A"/>
<!-- 获取接口及实现类 -->
<!-- <dubbo:reference interface="cn.itcast.core.service.TestTbService" id="testTbService" /> -->
<dubbo:reference interface="cn.itcast.core.service.TestTbService" id="testTbService" url="dubbo://127.0.0.1:20880"/>
```


2. 消费方设置超时时间        
在服务消费方设置超时时间
```xml
<!-- 全局统一设置请求超时时间，默认为1秒 ; 设置10分钟-->
<dubbo:consumer timeout="600000"/>
```

3. 消费方不检查注册中心及提供方的服务        
将 check 参数设置为 "false"，如下            
```xml
<!-- 注册中心 -->
<!-- <dubbo:registry address="192.168.1.110:2181" protocol="zookeeper" check="false"/> -->
<dubbo:registry address="N/A"/>
<!-- 获取接口及实现类 -->
<!-- <dubbo:reference interface="cn.itcast.core.service.TestTbService" id="testTbService" check="false"/> -->
<dubbo:reference interface="cn.itcast.core.service.TestTbService" id="testTbService" url="dubbo://127.0.0.1:20880" check="false"/>
```

