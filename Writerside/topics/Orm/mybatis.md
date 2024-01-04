# mybatis

## Mybatis原理
MyBatis是支持定制化SQL、存储过程以及高级映射的优秀的持久层框架。MyBatis避免了几乎所有的JDBC代码和手工设置参数以及抽取结果集。MyBatis使用简单的XML或注解来配置和映射基本体，将接口和Java的POJOs(Plain Old Java Objects, 普通的Java对象)映射成数据库中的记录。     
``原理详解：``      
MyBatis应用程序根据XML配置文件创建SqlSessionFactory，SqlSessionFactory在根据配置，配置来源于两个地方，一处是配置文件，一处是Java代码的注解，获取一个SqlSession。SqlSession包含了执行sql所需要的所有方法，可以通过SqlSession实例直接运行映射的sql语句，完成对数据的增删改查和事务提交等，用完之后关闭SqlSession。


## 什么是MyBatis?简述MyBatis的体系结构
1. API接口层：提供给外部使用的接口API，开发人员通过这些本地API来操纵数据库。接口层一接收到调用请求就会调用数据处理层来完成具体的数据处理。
2. 数据处理层：负责具体的SQL查找、SQL解析、SQL执行和执行结果映射处理等。它主要的目的是根据调用的请求完成一次数据库操作。
3. 基础支撑层：负责最基础的功能支撑，包括连接管理、事务管理、配置加载和缓存处理，这些都是共用的东西，将他们抽取出来作为最基础的组件。为上层的数据处理层提供最基础的支撑。

## Mybatis和Hibernate的异同点
``相同：``
1. Hibernate与MyBatis都可以是通过SessionFactoryBuider由XML配置文件生成SessionFactory，然后由SessionFactory生成Session，最后由Session来开启执行事务和SQL语句。其中SessionFactoryBuider，SessionFactory，Session的生命周期都是差不多的。
2.  Hibernate和MyBatis都支持JDBC和JTA事务处理。

``不同点：``
1. MyBatis可以进行更为细致的SQL优化，可以减少查询字段。
2. yBatis容易掌握，而Hibernate门槛较高。
3. ibernate的DAO层开发比MyBatis简单，Mybatis需要维护SQL和结果映射。
4. ibernate对对象的维护和缓存要比MyBatis好，对增删改查的对象的维护要方便。
5. Hibernate数据库移植性很好，MyBatis的数据库移植性不好，不同的数据库需要写不同SQL。
6. ibernate有更好的二级缓存机制，可以使用第三方缓存。MyBatis本身提供的缓存机制不佳。     
   
> [MyBatis和Hibernate的优缺点对比](https://www.cnblogs.com/javacatalina/p/6590321.html)


## 对于Hibernate和MyBatis的区别与利弊，谈谈你的看法 {id="hibernate-mybatis_1"}
1. hibernate真正掌握要比mybatis难，因为hibernate的功能和特性非常多，还不适合多表关联查询。
2. hibernate查询会将所有关联表的字段全部查询出来，会导致性能消耗，当然hibernate也可以自己写sql指定字段，但这就破坏了hibernate的简洁性。mybatis的sql是自己手动编写的，所以可以指定查询字段。
3. hibernate与数据库管联只需在xml文件中配置即可，所有的HQL语句都与具体使用的数据库无关，移植性很好；mybatis所有的sql都是依赖所用数据库的，所以移植性差。
4. hibernate是在jdbc上进行一次封装，mybatis是基于原生的jdbc，运行速度较快。
5. 如果有上千万的表或者单次查询或提交百万数据以上不建议使用hibernate。如果统计功能、多表关联查询较多较复杂建议使用mybatis。

## Hibernate和Mybatis的异同
相同点      
· Hibernate与MyBatis都可以是通过SessionFactoryBuider由XML配置文件生成SessionFactory，然后由SessionFactory 生成Session，最后由Session来开启执行事务和SQL语句。其中SessionFactoryBuider，SessionFactory，Session的生命周期都是差不多的。    
· Hibernate和MyBatis都支持JDBC和JTA事务处理。    
差异点      
全自动和半自动；    
因为Hibernate对查询对象有着良好的管理机制，用户无需关心SQL。所以在使用二级缓存时如果出现脏数据，系统会报出错误并提示；    
而MyBatis在这一方面，使用二级缓存时需要特别小心。如果不能完全确定数据更新操作的波及范围，避免Cache的盲目使用。否则，脏数据的出现会给系统的正常运行带来很大的隐患。         


## Mybatis比IBatis比较大的几个改进是什么
``ibatis3.x为Mybatis``
1. 有接口绑定,包括注解绑定sql和xml绑定Sql,接口映射就是在IBatis中任意定义接口,然后把接口里面的方法和SQL语句绑定,我们直接调用接口方法就可以,这样比起原来了SqlSession提供的方法我们可以有更加灵活的选择和设置.
2. 动态sql由原来的节点配置变成OGNL表达式,
3. 在一对一,一对多的时候引进了association,在一对多的时候引入了collection节点,不过都是在resultMap里面配置
4. 对象关系映射的改进，效率更高       ``一般实例化对象的几种方式：``new对象(常用)，使用反射newInstance创建对象，使用clcne方法，使用序列化和反序列化（使用对象流把对象实例输出到硬盘上到用的时候可以在取出来），动态代理（proxy 类 CGLIB）


## iBatis和MyBatis在细节上的不同有哪些
区别太多，篇幅太长，大家可以自行百度下。

## #{}和${}的区别是什么

${}是Properties文件中的变量占位符，它可以用于标签属性值和sql内部，属于静态文本替换，比如${driver}会被静态替换为com.mysql.jdbc.Driver。      

`#{}`是sql的参数占位符，Mybatis会将sql中的#{}替换为?号，在sql执行前会使用PreparedStatement的参数设置方法，按序给sql的?号占位符设置参数值，比如ps.setInt(0,parameterValue)，#{item.name}的取值方式为使用反射从参数对象中获取item对象的name属性值，相当于param.getItem().getName()。

--- 
1. ${}是Properties(pa pu tei si)文件中的变量占位符，它可以用于标签属性值和sql内部，属于静态文本替换
2. `#{}`是sql的参数占位符，Mybatis会将sql中的#{}替换为?号，在sql执行前会，按序给sql的?号占位符设置参数值

---

```sql
select * from location where id = ${id} => Select * from location where id = xxx;
Select * from location where id = #{} => Select * from location where id="xxxx";
```

${}方式无法防止sql注入;     
Order by动态参数时，使用${}而不用#{}    
模糊查询的时候使用${}

```sql
select * from location where name like '${}%';
```

## 什么是MyBatis的接口绑定,有什么好处 {id="mybatis_6"}
接口映射就是在IBatis中任意定义接口，然后把接口里边的方法和SQL语句绑定，我们可以直接调用接口方法，比起SqlSession提供的方法我们可以有更加灵活的选择和设置。

## 列举MyBatis的常用API及方法
org.apache.ibatis.session.SqlSession    
MyBatis工作的主要顶层API，表示和数据库交互的会话。完毕必要数据库增删改查功能。

org.apache.ibatis.executor.Executor     
MyBatis执行器，是MyBatis 调度的核心，负责SQL语句的生成和查询缓存的维护。

org.apache.ibatis.executor.statement.StatementHandler   
封装了JDBC Statement操作。负责对JDBC statement   的操作。如设置參数、将Statement结果集转换成List集合。

org.apache.ibatis.executor.parameter.ParameterHandler   
负责对用户传递的参数转换成JDBC Statement 所须要的参数。

org.apache.ibatis.executor.resultset.ResultSetHandler   
负责将JDBC返回的ResultSet结果集对象转换成List类型的集合

org.apache.ibatis.type.TypeHandler  
负责java数据类型和jdbc数据类型之间的映射和转换

org.apache.ibatis.mapping.MappedStatement   
MappedStatement维护了一条<select|update|delete|insert>节点的封装

org.apache.ibatis.mapping.SqlSource     
负责依据用户传递的parameterObject，动态地生成SQL语句，将信息封装到BoundSql对象中，并返回

org.apache.ibatis.mapping.BoundSql  
表示动态生成的SQL语句以及对应的参数信息

org.apache.ibatis.session.Configuration     
MyBatis全部的配置信息都维持在Configuration对象之中



## 最佳实践中，通常一个Xml映射文件，都会写一个Dao接口与之对应，请问，这个Dao接口的工作原理是什么？Dao接口里的方法，参数不同时，方法能重载吗？
Dao接口，就是人们常说的Mapper接口，接口的全限名，就是映射文件中的namespace的值，接口的方法名，就是映射文件中MappedStatement的id值，接口方法内的参数，就是传递给sql的参数。Mapper接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位MappedStatement，     
举例：com.mybatis3.mappers.StudentDao.findStudentById，可以唯一找到namespace为com.mybatis3.mappers.StudentDao下面id =findStudentById的MappedStatement。在Mybatis中，每一个`<select>`、`<insert>`、`<update>`、`<delete>`标签，都会被解析为一个MappedStatement对象。       
Dao接口里的方法，是不能重载的，因为是全限名+方法名的保存和寻找策略。      
Dao接口的工作原理是JDK动态代理，Mybatis运行时会使用JDK动态代理为Dao接口生成代理proxy对象，代理对象proxy会拦截接口方法，转而执行MappedStatement所代表的sql，然后将sql执行结果返回



## Mybatis的Xml映射文件中，不同的Xml映射文件，id是否可以重复？
可以。Mybatis namespace+id


## Mybatis都有哪些Executor执行器？它们之间的区别是什么？
SimpleExecutor执行一次sql，开始创建一个statement，用完关闭statement     
ReuseExecutor 类似数据库连接池，statement用完后返回，statement可以复用   
BatchExecutor主要用于批处理执行

## Mybatis是否可以映射Enum枚举类
可以。Mybatis可以映射枚举类，不单可以映射枚举类，Mybatis可以映射任何对象到表的一列上。

## mybatis如何实现能一对一和一对多的关联查询 {id="mybatis_2"}
1. Mybatis不仅可以执行一对一、一对多的关联查询，还可以执行多对一，多对多的关联查询，多对一查询，其实就是一对一查询，只需要把selectOne()修改为selectList()即可；多对多查询，其实就是一对多查询，只需要把selectOne()修改为selectList()即可。
2. 关联对象查询，有两种实现方式。一种是单独发送一个sql去查询关联对象，赋给主对象，然后返回主对象。另一种是使用嵌套查询，嵌套查询的含义为使用join查询，一部分列是A对象的属性值，另外一部分列是关联对象B的属性值，好处是只发一个sql查询，就可以把主对象和其关联对象查出来。
3. 那么问题来了，join查询出来100条记录，如何确定主对象是5个，而不是100个？其去重复的原理是resultMap标签内的id子标签，指定了唯一确定一条记录的id列，Mybatis根据id列值来完成100条记录的去重复功能，id可以有多个，代表了联合主键的语意。同样主对象的关联对象，也是根据这个原理去重复的，尽管一般情况下，只有主对象会有重复记录，关联对象一般不会重复。



##  Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？ {id="mybatis_3"}
Mybatis仅支持association关联对象和collection关联集合对象的延迟加载，association指的就是一对一，collection指的就是一对多查询。在Mybatis配置文件中，可以配置是否启用延迟加载lazyLoadingEnabled=true|false。     
它的原理是，使用CGLIB创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用a.getB().getName()，拦截器invoke()方法发现a.getB()是null值，那么就会单独发送事先保存好的查询关联B对象的sql，把B查询上来，然后调用a.setB(b)，于是a的对象b属性就有值了，接着完成a.getB().getName()方法的调用。这就是延迟加载的基本原理。      
当然了，不光是Mybatis，几乎所有的包括Hibernate，支持延迟加载的原理都是一样的



## Mybatis的动态标签? {id="mybatis_4"}
1. 用来循环容器的标签``forEach``,查看例子:      
   mapper中我们要为这个方法传递的是一个容器,将容器中的元素一个一个的拼接到xml的方法中就要使用这个forEach这个标签了

```java
public List<Entity> queryById(List<String> userids);
```

对应的xml中如下
```xml
<select id="queryById" resultMap="BaseReslutMap" >
    select * FROM entity where id in 
    <foreach collection="userids" item="userid" index="index" open="(" separator="," close=")">
        #{userid}
    </foreach>
</select>
```

2. concat模糊查询:      
   比如说我们想要进行条件查询,但是几个条件不是每次都要使用,那么我们就可以，通过判断是否拼接到sql中

```xml
<select id="queryById" resultMap="BascResultMap" parameterType="entity">
    SELECT * from entity
    <where>
        <if test="name!=null">
            name like concat('%',concat(#{name},'%'))
        </if>
    </where>
</select>
```

3. choose (when, otherwise)标签:        
   choose标签是按顺序判断其内部when标签中的test条件出否成立，如果有一个成立，则 choose 结束。当 choose 中所有 when 的条件都不满则时，则执行 otherwise 中的sql。类似于Java 的 switch 语句，choose 为 switch，when 为 case，otherwise 则为 default。        
   例如下面例子，同样把所有可以限制的条件都写上，方面使用。choose会从上到下选择一个when标签的test为true的sql执行。安全考虑，我们使用where将choose包起来，放置关键字多于错误。

```xml
<!-- choose(判断参数) - 按顺序将实体类 User 第一个不为空的属性作为：where条件 --> 
<select id="getUserList_choose" resultMap="resultMap_user" parameterType="com.yiibai.pojo.User"> 
    SELECT * FROM User u
    <where> 
        <choose> 
            <when test="username !=null "> 
                u.username LIKE CONCAT(CONCAT('%', #{username, jdbcType=VARCHAR}),'%') 
            </when > 
            <when test="sex != null and sex != '' "> 
                AND u.sex = #{sex, jdbcType=INTEGER} 
            </when > 
            <when test="birthday != null "> 
                AND u.birthday = #{birthday, jdbcType=DATE} 
            </when > 
            <otherwise> 
            </otherwise> 
        </choose> 
    </where> 
</select>
```

4. if标签:  
   if标签可用在许多类型的sql语句中，我们以查询为例。首先看一个很普通的查询：

```
<!-- 查询学生list，like姓名 --> 
<select id="getStudentListLikeName" parameterType="StudentEntity" resultMap="studentResultMap"> 
    SELECT * from STUDENT_TBL ST 
    WHERE ST.STUDENT_NAME LIKE CONCAT(CONCAT('%', #{studentName}),'%') 
</select>
```

但是此时如果studentName为null，此语句很可能报错或查询结果为空。此时我们使用if动态sql语句先进行判断，如果值为null或等于空字符串，我们就不进行此条件的判断，增加灵活性。参数为实体类StudentEntity。将实体类中所有的属性均进行判断，如果不为空则执行判断条件。


##  Mybatis是如何进行分页的？分页插件的原理是什么？ {id="mybatis_7"}
Mybatis使用RowBounds对象进行分页，它是针对ResultSet结果集执行的内存分页，而非物理分页，可以在sql内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。      
分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。  
举例：select * from student，拦截sql后重写为：select t.* from （select * from student）t limit 0，10

--- 

RowBounds针对ResultSet结果集执行分页

```
<select id="selectById" parameterType="int" resultType="com.dongnao.demo.dao.Location">
    select * from location where id = #{id}
</select>
```

插件分页    
@Intercepts     
分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。


## 简述Mybatis的插件运行原理，以及如何编写一个插件 {id="mybatis_5"}
Mybatis仅可以编写针对ParameterHandler、ResultSetHandler、StatementHandler、Executor这4种接口的插件。    
Mybatis使用JDK的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这4种接口对象的方法时，就会进入拦截方法，具体就是InvocationHandler的invoke()方法。当然，只会拦截那些你指定需要拦截的方法。      
实现Mybatis的Interceptor接口并复写intercept()方法，然后在给插件编写注解，指定要拦截哪一个接口的哪些方法即可。记住，别忘了在配置文件中配置你编写的插件。

## Mybatis动态sql是做什么的？都有哪些动态sql？简述一下动态sql的执行原理
Mybatis动态sql可以让我们在Xml映射文件内，以标签的形式编写动态sql，完成逻辑判断和动态拼接sql的功能。   
Mybatis提供了9种动态sql标签trim|where|set|foreach|if|choose|when|otherwise|bind        
原理为，使用OGNL从sql参数对象中计算表达式的值，根据表达式的值动态拼接sql，以此来完成动态sql的功能。       
动态SQL，主要用于解决查询条件不确定的情况：在程序运行期间，根据提交的查询条件进行查询。    
动态SQL，即通过MyBatis提供的各种标签对条件作出判断以实现动态拼接SQL语句。






## Mybatis的批量操作数据库? {id="mybatis_8"}
1. 批量插入insert:

```xml
<insert id="insertbatch" parameterType="java.util.List">
    <selectKey keyProperty="id" order="AFTER" resultType="int">
        SELECTLAST_INSERT_ID()
    </selectKey>
    INSERT INTO sourcedoc (
    sdate, sweek,
    roomno, daysched, nightsched,
    adminsched, vacationsched, programdept,
    programname
    )values
    <foreach collection="list" item="item" index="index" separator=",">
        (
        #{item.sdate},#{item.sweek},#{item.roomno},
        #{item.daysched},#{item.nightsched},#{item.adminsched},
        #{item.vacationsched},#{item.programdept},#{item.programname}
        )
    </foreach>
</insert>
```

2. 批量删除(delete):

```
<!-- 通过主键集合批量删除记录 -->
<delete id="batchRemoveUserByPks" parameterType="java.util.List">
    DELETE FROM LD_USER WHERE ID in
    <foreach item="item" index="index" collection="list" open="(" separator="," close=")">
        #{item}
    </foreach>
</delete>
```

3. 批量修改(update):

```
<updateid="updateOrders"parameterType="java.util.List">
    updateorderssetstate='0'wherenoin
    <foreachcollection="list"item="nos"open="("separator=","close=")">
    #{nos}
    </foreach>
</update>
```

## SSM运行流程：
1. jsp（view）发送请求
2. 通过核心控制器DispatcherServlet调用请求解析器：HandlendMapping对请求进行解析，通过映射关系匹配到Controller层
3. 在控制层调用业务逻辑层（service），数据持久层（DAO）返回控制层，请求完成获取一个结果，设置一个要跳转的视图，（ModelAndView装载并传输数据，设置视图）
4. 核心控制器调用 视图解析器：ViewResolver解析视图，匹配相应的页面实现页面跳转


## SSM整合的流程，SSM搭建步骤
在项目中通过在web.xml配置springMVC的核心控制器DispatcherServlet并加载Spring-mvc-controller.xml,并且通过配置Spring的监听器contextLoaderListener加载spring-common.xml，之后新建控制层并在类上加入@Controller和@RequestMapping注解，并通过@Resouce注入service层，   
在service的实现类上加入@Service注解并通过@Autowired注入dao层，dao层只有接口并没有实现类，是通过在mybatis中对应的含有sql语句的xml文件中来通过namespace指明要实现的dao层的接口，并使sql语句的id和dao层接口中的方法名一致从而明确调用指定dao层接口时要执行的sql语句。        
并且在spring-mvc-controller.xml中配置了component-scan对controller进行扫描从而使控制层的注解生效还配置了内部视图解析器从而在控制层进行页面跳转时加上指定的前缀和后缀，     
在spring-common.xml中配置了dbcp数据库连接池以及sqlSession来加载mapper下所有的xml并对所有的mapper层进行扫描也就是对dao层的扫描，还通过Aop中的切点表达式对service层进行事务控制，并且对service层进行扫描使其注解生效。


## myBatis {id="mybatis_1"}
mybatis是一个轻量级的持久层封装框架，对jdbc进行简单的封装，基于orm框架实现原理开发，相对于Hibernate框架来说，mybatis属于半自动持久层框架。      
目前mybatis在整体的web开发项目过程中，占比在70%以上，目前的项目整体都是大型的项目，在大项目的开发过程中，Hibernate完全体现不出开发效率高的价值，大型项目一般数据库表关系复杂，Hibernate不支持复杂的联查，还是需要把hql语句转换成sql语句来执行，而且大型的项目对于持久层优化反面比较注重，hql不利于做sql优化。        
mybatis的常用标签以及属性  
```
<resultMap></resultMap> 查询返回的结果集映射关系配置
<select></select> 查询标签
<insert></insert> 新增标签
<update></update> 修改标签
<delete></delete> 删除标签
<if test=""></if> 进行判断
id             相当于与实现类中的方法名，与接口中的方法名必须一致
resultMap      当前查询结果集对应的映射关系
parameterType  参数类型 前台页面或者Service层传递给dao层的参数类型，一般为实体类的类型
resultType     声明返回值类型 常用的有 int String map
```

## mybatis优势 {id="mybatis_9"}
1. 支持sql语句的优化，是程序员对sql语句的把控能力更大
2. 在整体的开发效率中与Hibernate对比，并没有相差太多
3. 相对于sql语句有很好的重复利用度
4. mybatis sql语句编写更加灵活，支持for循环 if esle 判断
5. 支持查询结果转换各种类型格式    
注：区别：$符号取值相当于sql语句中的拼接字符串，#号取值相当于占位符取值

