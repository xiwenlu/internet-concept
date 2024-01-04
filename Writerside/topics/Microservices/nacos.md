# nacos

## 配合maven实现环境的切换

1. bootstrap.properties中添加以下配置
```
spring.cloud.nacos.config.server-addr=@nacosServerAddr@
spring.cloud.nacos.config.username=@nacosUsername@
spring.cloud.nacos.config.password=@nacosPassword@
spring.cloud.nacos.config.namespace=@nacosNamespace@
spring.cloud.nacos.config.group=@nacosGroup@
spring.cloud.nacos.config.contextPath=/nacos
spring.cloud.nacos.config.refresh-enabled=true
spring.cloud.nacos.config.file-extension=properties
spring.cloud.nacos.config.prefix=${spring.application.name}
spring.cloud.nacos.config.extension-configs[0].data-id=gulimall-order.yml
spring.cloud.nacos.config.extension-configs[0].group=dev
spring.cloud.nacos.config.extension-configs[0].refresh=true
spring.cloud.nacos.config.extension-configs[1].data-id=gulimall-order.yml
spring.cloud.nacos.config.extension-configs[1].group=pro
spring.cloud.nacos.config.extension-configs[1].refresh=true


```
2. 在pom.xml中添加以下配置
```xml
<profiles>
    <profile>
        <id>dev</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <activatedProperties>dev</activatedProperties>
            <nacosNamespace>97b93b63-ee20-4464-a599-46494f9408a1</nacosNamespace>
            <nacosGroup>group-${activatedProperties}</nacosGroup>
            <nacosServerAddr>127.0.0.1:8848</nacosServerAddr>
            <nacosUsername>nacos</nacosUsername>
            <nacosPassword>nacos</nacosPassword>
        </properties>
<!--   prod、test 忽略     -->
    </profile>

<!--  此处  -->
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
</profiles>
```
3. 点击`reload all maven maven projects`按钮，刷新maven，将出现以下checked框，勾选进行环境切换

<img src="nacos-profiles.png" alt=""/>

<img src="nacos-reload.png" alt=""/>

