# redis应用

## spring整合redisTemplate
1. 我们的项目都是使用maven作为项目管理工具的。当maven与spring整合时，首先在项目的pom文件中增加相关jar包的依赖（Spring-data-redis.jar、jedis.jar），然后会在resource包下创建一个spring和redis的配置文件。
2. 在此配置文件中，首先配置jedis连接工厂，其次配置redis对应的主机地址和端口，然后配置jedis的连接池，把redis连接交给spring管理。spring会提供一个redisTemplate工具类让我们来操作redis，get方法取值，set方法存值，set方法有两种。第一个有两个参数，key 和value, 第二个set方法有三个参数，除了key和value，还有超时时间设置。
3. 我们还使用redis和spring的cacheManager进行了整合，在项目中通过@Cacheable注解缓存方法返回数据，key存的是对应的方法名和参数，下次访问此方法时就不会再走数据库查询了。

> 参考 [redis整合spring(redisTemplate工具类)](http://blog.csdn.net/qq_34021712/article/details/75949706)


## springBoot整合redisTemplate
1. 在pom.xml文件中添加SpringBoot Redis的依赖。
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
2. 在application.properties或application.yml文件中配置Redis的连接信息。
```yaml
spring:  
  redis:  
    host: your-redis-host  
    port: your-redis-port  
    password: your-redis-password (如果有密码的话)
```
3. 在配置类中创建RedisTemplate的Bean
```Java
@Configuration  
public class RedisConfig {  
    @Bean  
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {  
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();  
        redisTemplate.setConnectionFactory(redisConnectionFactory);  
        redisTemplate.setKeySerializer(new StringRedisSerializer()); // 设置key序列化器为String类型，也可以使用自定义序列化器。  
        return redisTemplate;  
    }  
}
```
4. 现在你可以在你的服务类中使用RedisTemplate进行操作了。

## spring缓冲注解

### @Cacheable
1. @Cacheable可以标记在一个方法上，也可以标记在一个类上。当标记在一个方法上时表示该方法是支持缓存的，当标记在一个类上时则表示该类所有的方法都是支持缓存的。        
2. 对于一个支持缓存的方法，Spring会在其被调用后将其返回值缓存起来，以保证下次利用同样的参数来执行该方法时可以直接从缓存中获取结果，而不需要再次执行该方法。        
3. Spring在缓存方法的返回值时是以键值对进行缓存的，值就是方法的返回结果。

### @CacheEvict
@CacheEvict是用来标注在需要清除缓存元素的方法或类上的。当标记在一个类上时表示其中所有的方法的执行都会触发缓存的清除操作。




## RedisAtomicInteger使用
RedisAtomicInteger是Redis中的一种原子操作类型，用于实现对整数的原子性操作。在多线程环境下，它能够确保在多个客户端对同一个整数进行读写操作时，不会发生数据竞争，从而保证了数据的一致性。
```java
    @Autowired
    private RedisTemplate redisTemplate;
    
    ...

    String ticketName = "testRedisAtomicInteger";
    RedisAtomicInteger redisCount = new RedisAtomicInteger(ticketName,redisTemplate.getConnectionFactory());

    ExecutorService pool = Executors.newFixedThreadPool(100);
    for (int i = 0; i < 100; i++) {
        pool.submit(() -> {
            for (int j = 0; j < 100; j++) {
                int count = redisCount.incrementAndGet();
                System.out.println(Thread.currentThread().getName()+": " +count);
            }
        });
    }

```

