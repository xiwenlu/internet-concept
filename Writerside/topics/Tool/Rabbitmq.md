# rabbitmq

## Rabbitmq消息队列 {id="rabbitmq_1"}
当初我们做短信发送的时候用到了rabbitmq消息队列，我们当时有一个短信发送平台，平台采用监听消息队列方式进行短信的发送，我们在项目中通过配置一个RebbitConfig在上面加@Configuration注解，然后在里边new一个Queue对象创建一个消息队列，然后我们再发送短信的地方注入AmqpTemplate,最后通过调用AmqpTemplate.convertAndSend将消息发送到队列当中，convertAndSend里边有两个参数，第一个是消息队列名称，第二个是消息队列发送进去的内容。短信发送平台通过RabbitListener queues等于队列名称，监听队列，通过RabbitHandler注解获取到消息队列中的数据，然后通过httpClient调用短信发送接口，将短信发送出去并将发送的数据以及发送状态记录到数据库中。


## 三．RabbitMQ如何保证消息进入队列
依据RabbitMQ和Spring AMQP参考文档，事务(Transactional)或发布确认(Publisher Confirms / aka Publisher Acknowledgements)机制可保证消息被正确投递，即从理论上来说MQ不会丢消息。          
注意这两种机制不能共存。事务机制是重量级的，同步的，会带来大量开销；发布确认机制则是轻量级的，异步的。         
对于发布确认机制，
1. 需置CachingConnectionFactory的publisherConfirms属性为true；
2. 生产者需调用setConfirmCallback(ConfirmCallback callback)，Confirms就会回调给生产者；
3. 消费者需考虑消息去重处理。

这里需要注意的是，一个RabbitTemplate只能支持一个ConfirmCallback
