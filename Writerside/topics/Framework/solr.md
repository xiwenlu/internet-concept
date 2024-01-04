# solr

## 说说Solr 或者lucence 如何使用solr？
上个项目中我在做房源搜索的时候用到了solr,solr是一个企业级的搜索技术，底层是基于lucence的，solr相比于普通的搜索的好处关键在于它的分词器，但是在国内solr对中文支持不是很好，所以我们一般需要借助第三方的分词器，当初在网上查阅资料后，我们选取了一个IK分词器，在创建solr索引的时候我们配置了一个dataimport，让solr直接和数据库连接起来，solr搜索的时候不会去数据库搜索而是在solr的索引库里边搜索，所以我们在里边配置上需要创建索引数据的查询sql，需要注意的是我们在创建索引的时候用了哪个分词器，在搜索的时候也得用那个分词器。      

···     
增量索引：我们在需要创建索引的表中会加一个时间字段，通过配置的deltaQuery根具时间查出上一次创建索引之后的数据，然后solr就会把这些查出来的数据创建成索引      
删除索引：删除索引的时候需要我们查到需要删除的数据的id,我们在需要创建索引的数据表中加入了一个逻辑删除字段，当我们需要删除索引的时候我们要先把数据库的逻辑字段状态给更改掉，然后通过配置的deletedPkQuery查找到逻辑删除的数据的主键id，然后solr就会帮我们把solr索引库中的索引给删除掉         
更新索引：更新索引其实就是走的增量索引，更新数据库数据的同时将那个时间字段也给更新了，然后调用solr接口通知solr增量更新索引。      

`solr与项目的整合使用:` 

1. 首先我们打开solr的服务器，在solr的server文件夹下面创建一个文件夹（文件夹名一般与业务相关），然后我们在这个文件夹中添加conf和data文件夹.
2. 然后我们导入ik分词的jar包，并在之前创建的conf文件夹中的managered-schema配置有关ik分词器的标签。
3. 然后我们获取mysql连接包,在conf文件夹中配置data-config.xml文件,文件当中配置数据库的链接地址、query、deltaQuery增量索引、deltaImportQuery、deletedPkQuery删除索引这些。
4. 我们在managered-schema中添加查询出的字段对应的分词器。并在solrconfig.xml中配置加载data-config.xml文件和引入jar包（具体配置lib）      

这样solr索引库与数据库链接创建好了。    
接着就是与项目的真正结合。   
`整合：`   
1. 首先导入solrj pom依赖:
```xml
<!-- sorlj-->
<dependency>
   <groupId>org.apache.solr</groupId>
   <artifactId>solr-solrj</artifactId>
   <version>4.10.2</version>
</dependency>
```

2. 然后我们通过@RequestMapping来请求后台方法，搜索对应索引库中的数据。
3. 具体实现就是
```
SolrServer solrServer = new HttpSolrServer("http://localhost:8983/solr/house");
```

参数为solr的访问地址，然后new SolrQuery(); new SolrQuery(); 通过返回对象进行查询
