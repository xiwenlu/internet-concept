# tomcat

## tomcat优化 {id="tomcat_1"}
tomcat优化我们主要从三方面优化，
1. 是内存优化，tomcat内存优化主要是对tomcat启动参数优化，我们可以在 tomcat 的启动脚catalina.sh 中设置 java_OPTS 参数，通过Xms来初始化虚拟机最小内存Xmx来初始化虚拟机可使用的最大内存，
2. 是并发优化，通过配置server.xml里的Connector标签，在标签里配置maxThreads客户请求最大线程数、minSpareThreads 初始化时创建的socket线程数maxSpareThreads连接器的最大空闲socket线程数，connectionTimeout 连接的超时时间，minProcessors服务器创建时的最小处理线程数，maxProcessors服务器同时最大处理线程数，
3. 是缓存优化，配置compression打开压缩功能

## tomcat默认端口号
8080
