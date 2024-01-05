# zookeeper

## zookeeper在这里边起什么作用？zookeeper能被别的产品替换吗？
作用
· 数据发布与订阅（配置中心） 
· 负载均衡  
· 命名服务  
· 分布式通知/协调  
· 集群管理与Master(ma si de er)选举    
· 分布式锁      
· 分布式队列     
Zookeeper可以被Dubbo 自带的multicast或者redis来替代    
multicast,广播受到网络结构的影响，一般本地不想搭注册中心的话使用这种调用   
redis通常和Zookeeper相辅相成来使用    
两套东西，相辅相成，一般配合使用，举个例子，说明zookeeper如何配合redis使用。   
在redis实际应用中，一般会有一个主节点redis，几个从属节点redis，主节点处理用户写请求，从节点向主节点同步数据，处理用户读的请求。当主节点故障的时候，从节点会通过raft算法选举出新的主节点处理用户写请求。       

## 问题产生:如何在主服务器变更后，更新项目redis读写配置？      
原始做法: 修改项目代码里面的redis地址和端口，然后上传代码，再重新部署      
Zookeeper解决方法:通过zookeeper监控主从服务器变动，在项目里面引入zookeeper客户端，当主服务器变更的时候，zookeeper客户端会收到通知，在通知回调里，我们可以收到新的主节点地址和端口，根据他写一段redis重启代码，这样就避免了服务器重启
