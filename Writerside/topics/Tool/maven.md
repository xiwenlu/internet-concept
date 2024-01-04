# maven

## bootstrap.yml和application.yml区别

1. bootstrap.yml文件也是Spring Boot的默认配置文件，而且其加载的时间相比于application.yml更早
2. application.yml和bootstrap.yml虽然都是Spring Boot的默认配置文件，但是定位却不相同
3. bootstrap.yml可以理解成系统级别的一些参数配置，这些参数一般是不会变动的
4. application.yml可以用来定义应用级别的参数，如果搭配 spring cloud config 使用，application.yml里面定义的文件可以实现动态替换

总结就是：
1. bootstrap.yml文件相当于项目启动时的引导文件，内容相对固定。application.yml文件是微服务的一些常规配置参数，变化比较频繁
2. bootstrap.yml先于application.yml

## maven引入第三方jar包
```
 <dependency>
     <groupId>com.xx.x.xx</groupId>
     <artifactId>RXTXcomm</artifactId>
     <version>3.0.1</version>
     <scope>system</scope>
     <systemPath>${project.basedir}/libs/XXX.jar</systemPath>
 </dependency>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <compilerArguments>
            <extdirs>${project.basedir}/libs</extdirs>
        </compilerArguments>
    </configuration>
</plugin>
```

## additional-spring-configuration-metadata.json自定义提示
1. maven添加
```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-configuration-processor</artifactId>
   <optional>true</optional>
</dependency>
```
2. idea设置中搜索Annotation Processors，勾住Enable annonation processing
3. 启动项目之后会在out下面生成 META-INF 文件夹

   <img src="maven.png" alt=""/>
4. 把 META-INF 拖到 resources 下面

   <img src="maven2.png" alt=""/>

## 在pom中添加阿里云镜像仓库
```
<repositories>
    <repository>
        <id>public</id>
        <name>aliyun nexus</name>
        <url>https://maven.aliyun.com/nexus/content/groups/public/</url>
        <releases>
            <enabled>true</enabled>
        </releases>
    </repository>
</repositories>

<pluginRepositories>
    <pluginRepository>
        <id>public</id>
        <name>aliyun nexus</name>
        <url>https://maven.aliyun.com/nexus/content/groups/public/</url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </pluginRepository>
</pluginRepositories>
```

## 你们公司的maven私服怎么搭建的
因为经常需要自行编译，每次从maven下载依赖都是一件很头疼的事情，而且不同的网络环境速度也不一样，因此在自己的笔记本（windows 64位）上自行搭建一个nexus oss maven仓库，是一件很必要的事情，本文记录了我搭建全部过程，以及遇到的全部问题：      
下载          
http://www.sonatype.org/nexus/      
nexus-2.11.4-01-bundle.tar.gz           
注意下载bundle版本的       
安装      
解压后会有两个文件夹：nexus-2.11.4-01和sonatype-work，前者包含了运行环境和应用程序，后者是配置和数据。           
注意要以管理员方式打开命令提示符：       
进入目录：\nexus-2.11.4-01\bin\jsw\windows-x86-64        
运行：     
install-nexus.bat       
然后运行：       
start-nexus.bat结果如下所示：      
wrapper  | Starting the nexus service...        
wrapper  | Waiting to start...      
wrapper  | Waiting to start...      
wrapper  | nexus started.       
然后访问：http://localhost:8081/nexus/
1. 登录:首先点击右上角进行登录：账号：admin 密码 ： admin123
2. 配置仓库:登录后，点击左侧Repositories，默认会显示一些仓库，标红的这一行public Repository就是我们配的私服，注意他的Type是group，意思就是：所有请求设定仓库的request都会转向我们假设的私服。 在public Repository的Configuration中，左侧的就是我们所设定的仓库，如果我们在项目中设定了这些仓库，那么所有发向这些仓库的请求都会先访问们的私服，如果我们的私服中有想要的jar包等，就直接从私服下载。
3. 验证联通:我们点击Repositories中的Central，看看是否能联通，在Routing界面中，查看仓库状态是否成功：如果未成功，看看你是否在代理环境下，如果是的话，请设置代理。在左侧菜单栏，Administration/Server中设置：
4. index下载:首先在central中的Configuration中，将Downlad Remote Indexes设置为True。然后右键Central，Repair Index。同样，在Public Repositories，也右键进行Repair。这个时候，我们可以通过查看Scheduled Tasks，看到有一个Task正在运行。稍等片刻，若成功的话。可在Browse Index中查看到列表：       
   如果你的列表是空的话等一等，因为文件比较大，再加上咱们伟大的墙，所以有时候会很久没反映。（实在不行翻一下吧）,下载的文件可在：sonatype-work\nexus\indexer中找到
5. 连接私服     
   终于到了最后一步了，现在我们在项目的pom.xml中加入如下内容：

```xml
<repositories>
    <repository>
        <id>public</id>
        <name>Public Repositories</name>
        <url>http://localhost:8081/nexus/content/groups/public/</url>
    </repository>
</repositories>
```


这样我们再刷新项目的时候，就会从私服中下载了，若私服中没有则会从我们之前设置的Ordered Group Repositories从第一个开始寻找，因此我们要将最常用最快的仓库放在第一个。
下载成功后，我们可以在ui中看到：
当然了你也可以配置全局的，而不需要每个项目都配置，那就是在setting.xml中配置mirror。setting.xml在maven的安装目录的conf目录中（全局），或者在用户的家目录.m2/下（用户特定），在配置文件中加入：
```xml
<mirrors>
    <mirror>
        <id>bbq</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus osc bbq</name>
        <url>http://localhost:8081/nexus/content/groups/public/</url>
    </mirror>
</mirrors>
```
尽情享受高速的maven镜像吧！


## maven私服搭建 {id="maven_1"}
这个很简单啊，我们公司用的是nexus这个东西，搭建也比较简单，系统里配上jdk环境变量，然后直接启动nexus服务，由于nexus默认连接的是maven中央库，受限于网速的原因，有时候下载jar包较慢，所以我们在nexus私服上配置第三方代理仓库，开源中国仓库已经停止了，我们配的是阿里云的仓库，nexus创建连接服务有几个选项：公共组、代理、第三方等，配置完阿里云的仓库代理，还要把这个代理添加到公共组中，然后在maven的settings.xml文件中配置mirror镜像，我们就是这么用的。


## Maven中的scope总结
1. runtime 是运行的意思。指的是直接在运行时所需要的包，而非在编译时等时候需要的包。
2. \<optional>true\</optional> 父模块A的依赖中添加\<optional>true\</optional>，显式调用joda-time的jar包依赖，在这种情况下，在子模块B中会正常引用joda-time依赖jar包，对于\<optional>true\</optional>来说，父模块中是否加上\<dependencyManagement>都一样，都是需要显示调用才能引用joda-time的jar包依赖




