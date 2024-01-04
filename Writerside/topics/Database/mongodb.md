# mongodb

## mongodb是什么 {id="mongodb_2"}
1. MongoDB是由C++语言编写的，是一个基于分布式文件存储的开源数据库。
2. MongoDB是NOSQL（Not Only Sql）的非关系型数据库，易于扩展，可以进行分布式文件存储，适用于大数据量、高并发、弱事务的互联网应用。
3. mongodb中有三元素：数据库（db），集合（collection），文档（document），其中“集合”就是对应关系数据库中的“表”，“文档”对应“行”。
4. MongoDB将数据存储为一个文档，数据结构由键值对组成。 
5. MongoDB文档类似于json的bson（BinaryJSON）格式。字段值可以包含其他文档，数组及文档数组。
6. MongoDB服务端可运行在Linux、Windows或macos平台，支持32位和64位应用，默认端口为27017。推荐运行在64位平台，因为MongoDB在32位模式运行时支持的最大文件尺寸为2GB。

## 为什么要用mongodb {id="mongodb_3"}
它的特点是高性能、易部署、易使用，存储数据非常方便。  
主要功能特性有：  
*面向集合存储，易存储对象类型的数据。  
*模式自由。  
*支持动态查询。  
*支持完全索引，包含内部对象。  
*支持查询。  
*支持复制和故障恢复。  
*使用高效的二进制数据存储，包括大型对象（如视频等）。  
*自动处理碎片，以支持云计算层次的扩展性。  
*支持RUBY，PYTHON，JAVA，C++，PHP，C#等多种语言。  
*文件存储格式为BSON（一种JSON的扩展）。  
*可通过网络访问

## spring和mongodb整合步骤
添加依赖的jar包     
定义实体类      
Spring配置          
新建application.xml配置文件     
然后编写操作mongodb的接口       
再写对应接口的实现类：          
测试类开始进行测试

## mongodb适合应用在那些场景 {id="mongodb_4"}
mongodb是非关系型数据库，他的存储数据可以超过上亿条（老版本的mongodb有丢数据的情况，新版本不会有，网上说的），mongodb适合存储 一些量大表关系较简单的数据。          
从目前阿里云 MongoDB 云数据库上的用户看，MongoDB的应用已经渗透到各个领域，比如游戏、物流、电商、内容管理、社交、物联网、视频直播等，以下是几个实际的应用案例。
1. 游戏场景，使用 MongoDB 存储游戏用户信息，用户的装备、积分等直接以内嵌文档的形式存储，方便查询、更新。
2. 物流场景，使用 MongoDB 存储订单信息，订单状态在运送过程中会不断更新，以 MongoDB 内嵌数组的形式来存储，一次查询就能将订单所有的变更读取出来。
3. 社交场景，使用 MongoDB 存储存储用户信息，以及用户发表的朋友圈信息，通过地理位置索引实现附近的人、地点等功能。
4. 物联网场景，使用 MongoDB 存储所有接入的智能设备信息，以及设备汇报的日志信息，并对这些信息进行多维度的分析。
5. 视频直播，使用 MongoDB 存储用户信息、礼物信息等

## mongodb应用场景介绍
其主要适应场景如下：  
1）网站实时数据处理。它非常适合实时的插入、更新与查询，并具备网站实时数据存储所需的复制及高度伸缩性。  
2）缓存。由于性能很高，它适合作为信息基础设施的缓存层。在系统重启之后，由它搭建的持久化缓存层可以避免下层的数据源过载。   
3）高伸缩性的场景。非常适合由数十或数百台服务器组成的数据库，它的路线图中已经包含对MapReduce引擎的内置支持。  
不适用的场景如下：  
1）要求高度事务性的系统。  
2）传统的商业智能应用。  
3）复杂的跨文档（表）级联查询。

## mongodb常用命令 {id="mongodb_1"}


```

#mongodb启动命令
./mongod --dbpath=数据存储地址 --logpath=日志存储地址

#查询所有数据库
show dbs		
#显示当前数据库中的集合（类似关系数据库中的表）
show collections
#显示用户
show users		
#选择某个库使用 没有库时自动创建
use dbname	
#删除当前使用的数据库
db.dropDatabase() 	
#查看数据库
db.getName() 或 db 		
#查看数据库状态
db.stats() 		
#查看mongodb版本
db.version() 
#查询student表下的所有数据 相当于select * from student
db.student.find(); 	
#格式化输出的json
db.student.find().pretty();
#查找表中第一条数据
db.student.findOne(); 		
db.student.find().limit(100);
#统计当前表的数量，相当于 select count(1) from student;
db.student.find({ }).count(); 	
#跳过第一条查询后50条
db.student.find({ }).skip(1).limit(50); 
#去除出name重复的数据，相当于select distict name from userInfo;
db.userInfo.distinct("name"); 
#带条件查询 查询年龄等于6岁的，相当于 select * from student where age = 6;
db.student.find({"age":"6"}) 		
#查询大于10岁的学生 {"age":{$gt:10}}
db.student.find({"age":{$gt:10}}) 		
#年龄范围查询 查询大于10小于30岁的学生
db.student.find({"age":{$gt:10,$lt:30}})	
#多条件查询 名字叫zhangsan年龄6岁的学生
db.student.find({"name":"zhangsan","age":6})；	
#相当于select * from student where age =6 or name = 'lsi';
db.student.find({$or:[{"age":6},{"name":"lsi"}]})；	
#模糊查询 查询名字中带有s的人 相当于 select * from student where name like '%s%'; 相当于 %%
db.student.find({"name":/s/}) 
#查看姓zhang的学生 相当于 select *　from student where name like 'zhang%'; ^标识一什么开头 相当于js正则中的用法
db.student.find({"name":/^zhang/})
#查询名字san结尾的学生 相当于 select * from student where name like '%san'; $相当于 js正则中的用法
db.student.find({"name":/san$/})
#按照年龄正着排序 相当于 select * from student order by age asc;
db.student.find().sort({"age":1}); 
#按照年龄倒着排序 相当于 select * from student order by age desc;
db.student.find().sort({"age":-1}); 
db.student.ensureIndex({"name":1});          #创建索引
db.student.totalIndexSize();                 #查看总索引记录大小
db.student.getIndexes();                     #查询当前聚集集合所有索引
##修改
db.collection.update(criteria, objNew, upsert, multi )
#criteria:update的查询条件，类似sql update查询内where后面的
#objNew:update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的。
#upsert : 如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
#multi : mongodb默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
#相当于 update student set name='zhaoliu' where age = 6;
db.student.update({"age":6},{$set:{"name":"zhaoliu"}},false,true); 
#相当于 update student set age = age+10 where age = 6;
db.student.update({"age":6},{$inc:{"age":10}},false,true); 
##删除
#删除年龄等于26的学生信息 相当于 delete student where age = 26
db.student.remove({age:26}) 
#增加列
db.student.update({},{$set:{score:0}},true,true)
#删除列
db.student.update({},{$unset:{score:0}},true,true)
```


| 函数   | 释义   |
|------|------|
| $gt  | 大于   |
| $lt  | 小于   |
| $gte | 大于等于 |
| $lte | 小于等于 |
| $ne  | 不等于  |

## mongodb在项目中怎么使用的，代码怎么写的，怎么和spring整合的
我们主要用mongodb来存储我们项目里面的操作日志和课程评论日志记录的话我们主要是结合aop来使用的，首先我们来配置一个aop的切面类，再给aop的使用规则，哪个类里面的哪个方法使用当前切面类，利用后置通知类获取当前方法的操作日志，将操作日志存储到mongodb，然后进行分类管理查看。利用后置通知传到数据库。评论的话我们做的是一个树表结构，就是用户可以回复用户的评论，主要字段有（评论id,课程id,标题，内容，评论人，评论的发布时间）并且为了提高可用性和高并发用了3台服务器做了mongodb的副本集，其中一台作为主节点，另外两台作为副本节点，这样在任何一台mongodb服务器宕机时就会自动进行故障转移，不会影响应用程序对mongodb的操作，为了减轻主节点的读写压力过大的问题，我还对mongodb副本集做了读写分离，使写操作在主节点进行，读取操作在副本节点进行。为了控制评论，我们的评论的界面设置在了购买状态，只有购买了该课程的人才可以评论      

怎么整合        
首先添加spring和mongodb整合的jar包spring-data-commons，在spring的配置文件中引入mongodb的标签头,在配置文件内添加mongo 标签配置链接mongodb数据库的地址和端口号，然后注入mongodb的mongoTemplate模板，在模板里通过constructor-arg ref 指明上面配置好的mongo数据库连接信息，通过constructor-arg name 指明要操作的数据库，最后将我们的mongoTemplate模板注入到我们需要操作mongodb数据的Dao层中。            


## 你们的mongodb主要在什么场景下使用？2条以上，并介绍spring和mongodb 如何整合的。整合步骤5条以上。
从目前阿里云 MongoDB 云数据库上的用户看，MongoDB的应用已经渗透到各个领域，比如游戏、物流、电商、内容管理、社交、物联网、视频直播等，之所以采用mongodb是因为:MongoDB是NoSQL的非关系型数据库，他的存储数据可以超过上亿条（老版本的mongodb有丢数据的情况，新版本不会有，网上说的），mongodb适合存储 一些量大表关系较简单的数据,易于扩展，可以进行分布式文件存储，适用于大数据量、高并发、弱事务的互联网应用，

`以下是几个实际的应用案例:`
游戏场景，使用MongoDB存储游戏用户信息，用户的装备、积分等直接以内嵌文档的形式存储，方便查询、更新        
物流场景，使用MongoDB存储订单信息，订单状态在运送过程中会不断更新，以MongoDB内嵌数组的形式来存储，一次查询就能将订单所有的变更读取出来。     
社交场景，使用MongoDB存储存储用户信息，以及用户发表的朋友圈信息，通过地理位置索引实现附近的人、地点等功能        
物联网场景，使用MongoDB存储所有接入的智能设备信息，以及设备汇报的日志信息，并对这些信息进行多维度的分析     
视频直播，使用MongoDB存储用户信息、礼物信息等      

`在我们实际项目中：`     

1. 我们主要用mongodb来存储我们项目里面的操作日志（银行的付款转账记录，角色权限的变动日志），我们主要是结合aop来使用的，首先我们来配置一个aop的切面类，再给aop的使用规则，哪个类里面的哪个方法使用当前切面类，利用后置通知类获取当前方法的操作日志，将操作日志存储到mongodb，然后进行分类管理查看。利用后置通知传到数据库。
2. 我们在项目中使用它来存储电商产品详情页的评论信息（评论id,商品id,标题，评分，内容，评论人信息，评论的发布时间，评论标签组）并且为了提高可用性和高并发用了3台服务器做了mongodb的副本集，其中一台作为主节点，另外两台作为副本节点，这样在任何一台mongodb服务器宕机时就会自动进行故障转移，不会影响应用程序对mongodb的操作，为了减轻主节点的读写压力过大的问题，我还对mongodb副本集做了读写分离，使写操作在主节点进行，读取操作在副本节点进行。为了控制 留言，我们留言的界面设置在了订单状态，只有状态为5，也就是交易成功收货后才能评论，并在评论成功后将订单状态改为6
   
【补充：如果问到副本集是怎么搭建的，就说我们有专门的运维人员来负责搭建，我只负责用Java程序去进行操作】
1. 添加依赖的jar包， 定义实体类 , Spring配置, 新建application.xml配置文件, 引入mongodb标签, 链接mongodb客服端,编写操作mongodb增删查改的接口, 再写对应接口的实现类, 实现类注入到service层调用
2. 要想整合Spring和Mongodb必须下载相应的jar，这里主要是用到两种jar一种是spring-data-document和spring-data-commons两种类型的jar 
3. 定义实体类
4. Spring配置         
   新建application.xml配置文件，这个必须要的引入mongodb标签xmlns:mongo="http://www.springframework.org/schema/data/mongo"        
在配置文件中加入链接mongodb客服端
```
<mongo:mongo host="localhost" port="27017"></mongo:mongo>
```
注入mogondb的bean对象
```
<bean id="mongoTemplate" class="org.springframework.data.document.mongodb.MongoTemp">  
    <constructor-arg ref="mongo"/>  
    <constructor-arg name="databaseName" value="db"/>  
    <constructor-arg name="defaultCollectionName" value="person" />  
</bean>  

<bean id="personRepository" class="com.mongo.repository.PersonRepository">  
    <property name="mongoTemplate" ref="mongoTemplate"></property>  
</bean>

```

5. 然后编写操作mongodb增删查改的接口
```java
public interface AbstractRepository {  
    public void insert(Person person);  
    public Person findOne(String id);  
    public List<Person> findAll();  
    public List<Person> findByRegex(String regex);  
    public void removeOne(String id);  
    public void removeAll();  
    public void findAndModify(String id);  
}
```
再写对应接口的实现类：
```java 
import java.util.List;  
import java.util.regex.Pattern;  
import org.springframework.data.document.mongodb.MongoTemplate;  
import org.springframework.data.document.mongodb.query.Criteria;  
import org.springframework.data.document.mongodb.query.Query;  
import org.springframework.data.document.mongodb.query.Update;   
import com.mongo.entity.Person;  
import com.mongo.intf.AbstractRepository;

public class PersonRepository implements AbstractRepository{  

    private MongoTemplate mongoTemplate;  
    
    @Override  
    public List<Person> findAll() {  
        return getMongoTemplate().find(new Query(), Person.class);  
    }  
    
    @Override  
    public void findAndModify(String id) {   
        getMongoTemplate().updateFirst(new Query(Criteria.where("id").is(id)), new Update().inc("age", 3));
    } 
     
    @Override  
    public List<Person> findByRegex(String regex) {  
        Pattern pattern = Pattern.compile(regex,Pattern.CASE_INSENSITIVE);  
        Criteria criteria = new Criteria("name").regex(pattern.toString());            
        return getMongoTemplate().find(new Query(criteria), Person.class);        
    }  
    
    @Override  
    public Person findOne(String id) {  
        return getMongoTemplate().findOne(new Query(Criteria.where("id").is(id)),Person.class);  
    }  
    
    @Override  
    public void insert(Person person) {  
        getMongoTemplate().insert(person);  
    }  
    
    @Override  
    public void removeAll() {  
        List<Person> list = this.findAll();  
        if(list != null){  
            for(Person person : list){  
                getMongoTemplate().remove(person);  
            }  
        }  
    } 
     
    @Override  
    public void removeOne(String id){  
        Criteria criteria = Criteria.where("id").in(id);  
        if(criteria == null){  
            Query query = new Query(criteria);  
            if(query != null && getMongoTemplate().findOne(query, Person.class) != null)  
                getMongoTemplate().remove(getMongoTemplate().findOne(query, Person.class));            
        }  
    }
    
    public MongoTemplate getMongoTemplate() {  
        return mongoTemplate;  
    } 
     
    public void setMongoTemplate(MongoTemplate mongoTemplate) {  
        this.mongoTemplate = mongoTemplate;  
    }
}


```

6. 实现类注入到service层调用

## Spring中Mongodb的java实体类映射

spring-data-mongodb中的实体映射是通过MongoMappingConverter这个类实现的。它可以通过注释把java类转换为mongodb的文档。    
`它有以下几种注释：`      
@Id - 文档的唯一标识，在mongodb中为ObjectId，它是唯一的，通过时间戳+机器标识+进程ID+自增计数器（确保同一秒内产生的Id不会冲突）构成。    
@Document - 把一个java类声明为mongodb的文档，可以通过collection参数指定这个类对应的文档。     
@DBRef - 声明类似于关系数据库的关联关系。ps：暂不支持级联的保存功能，当你在本实例中修改了DERef对象里面的值时，单独保存本实例并不能保存DERef引用的对象，它要另外保存，如下面例子的Person和Account。      
@Indexed - 声明该字段需要索引，建索引可以大大的提高查询效率。      
@CompoundIndex - 复合索引的声明，建复合索引可以有效地提高多字段的查询效率。     
@GeoSpatialIndexed - 声明该字段为地理信息的索引。    
@Transient - 映射忽略的字段，该字段不会保存到mongodb。     
@PersistenceConstructor - 声明构造函数，作用是把从数据库取出的数据实例化为对象。该构造函数传入的值为从DBObject中取出的数据。     

spring data 4 mongoDB自动创建复合索引       
spring data 4 mongodb 在domain上添加annation，自动创建复合索引时需要使用CompoundIndexes。        
例如：      
`@CompoundIndex(name = "shop_index", def = "{platform : 1, shopId : 1}")`
程序也不会有编译错误或者执行错误，但是spring data不会建立任何索引,下面这样写才会启动时自动建立复合索引。     
```java
@CompoundIndexes({
   @CompoundIndex(name = "shop_index", def = "{platform : 1, shopId : 1}")
}) 
```
> 参考：[Spring中Mongodb的java实体类映射](https://blog.csdn.net/u010084868/article/details/52622624)

