# SpringBoot

## 你给我说说springBoot

springBoot 致力于简洁快速创建一个基于 spring 的项目，让开发者写更少的配置，程序能够更快的运行和启动。并且它是 springCloud（微服务）的基础。在 resources 文件下下有一个 application.yml 文件，在里面写程序的配置文件。运行 Application的main(),呈现会启动。
1. 独立运行的 spring项目，springBoot 可以以 jar 包的形式来运行，运行一个 springBoot 项目我们只需要通过 `java -jar xx.jar` 类运行。非常方便。
2. 内嵌Servlet容器。springBoot 可以内嵌 tomcat，这样我们无需以war包的形式部署项目。
3. 提供 starter 简化 maven 配置，使用 spring 或者 springMVC 我们需要添加大量的依赖，而这些依赖很多都是固定的，这里 springBoot 通过 starter 能够帮助我们简化 maven 配置。
4. 自动配置 spring
5. 准生产的应用监控
6. 无代码生成和xml配置。
   
springBoot 的核心思想就是约定大于配置，一切自动完成。 
