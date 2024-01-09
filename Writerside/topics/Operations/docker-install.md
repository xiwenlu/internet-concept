# docker

## docker常用命令
```Shell

# 查看容器的ip
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id

# 查看日志
docker logs -f --tail=100 921

# 进入容器
sudo docker exec -it 775c7c9ee1e1 /bin/bash  

# 从容器复制到宿主机
docker cp redis:/data/appendonly.aof /home/docker/redis/conf

# 从宿主机复制到容器
docker cp /home/docker/redis/conf/appendonly.aof redis:/data

```

## Dockerfile

### 构建nodejs项目
```
FROM alpine:latest
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
RUN apk add --no-cache --update nodejs nodejs-npm
COPY . /home/www/express
RUN cd /home/www/express && npm install --production
WORKDIR /home/www/express
EXPOSE 3000
ENTRYPOINT ["npm", "run"]
CMD ["start"]
```

## 安装elasticsearch
```Shell
docker run --name elasticsearch -p 9200:9200  -p 9300:9300 \
-e "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms84m -Xmx512m" \
-v /opt/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /opt/elasticsearch/data:/usr/share/elasticsearch/data \
-v /opt/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.12.0

# 启动 kibana
docker run  --name kibana -p 5601:5601  -d kibana:7.12.0

# elasticsearch-head
docker run -d  --name elasticsearch-head -p 9100:9100 docker.io/mobz/elasticsearch-head:5

```

## hadoop相关
### 安装spark
```Shell
docker run -it --name my-spark -p 4040:4040 apache/spark /opt/spark/bin/spark-shell
```

## mq安装
### 安装rabbitmq

```Shell
# -d 表示后台运行
# -p 表示端口映射
docker run -d --hostname my-rabbit --name rabbit -p 15672:15672 -p 5672:5672 rabbitmq:latest

# 容器内部。装载可视化插件
rabbitmq-plugins enable rabbitmq_management

# 容器内部。列出所有的用户
rabbitmqctl list_users

# 容器内部。设置新用户
rabbitmqctl add_user admin 123456

# 容器内部。修改账号admin的密码
rabbitmqctl  change_password  admin  '1234567'

# 容器内部。设置用户为最高操作权限
rabbitmqctl set_user_tags admin administrator 

# 容器内部。启用rabbitmq_management（访问控制台）
rabbitmq-plugins enable rabbitmq_management

# 重启容器
docker restart my-rabbit
```

访问：http://localhost:15672，输入创建好的账号密码登录


### 安装activemq
```Shell
docker run --name='activemq' \
            -itd \
            -p 8161:8161 \
            -p 61616:61616 \
            -e ACTIVEMQ_ADMIN_LOGIN=admin \
            -e ACTIVEMQ_ADMIN_PASSWORD=123456 \
            --restart=always \
            webcenter/activemq:latest
```

## 什么是Docker Extensions？
Docker Extensions是一个允许开发人员在Docker Desktop中构建新功能、扩展现有功能并发现和集成其他工具的扩展系统。它通过使用第三方工具（如插件）来帮助开发人员扩展Docker的功能，从而提高工作效率和工作流的顺畅性。

### Disk Usage扩展
你可以立即了解Docker资源在计算机上使用了多少磁盘空间，并通过删除特定类型的资源（如停止的容器、悬挂的镜像和未使用的卷）来回收空间。

### Tailscale扩展
Tailscale扩展允许你在Docker Desktop上完成这项工作。Tailscale私下几乎立即共享你的容器，而无需进行任何网络设置。它检测所有暴露端口的容器，并使其可供你的私人Tailscale网络中的人员使用。属于你的Tailscale网络的任何人都可以看到你的容器。

### Logs Explorer扩展
Logs Explorer扩展提供了一个可以同时浏览正在运行和停止的容器中的日志、使用正则表达式对其进行过滤以及使用粘性搜索过滤器的地方。

### Slim.AI扩展
它使你能够深入了解本地镜像的组成，如文件系统、元数据、图层信息等。这有助于识别容器中的实用程序（例如，curl），并探索其中包含的任何基于文本的文件（如配置、shell脚本、REASME）。

### docker extension命令
```Shell
(base) xiwenlu@xiwenludeMac-mini ~ % docker extension help
Usage:  docker extension [OPTIONS] COMMAND

Manages Docker extensions

Options:
      --socket string   The Desktop extension manager socket

Management Commands:
  dev             Extension development helpers

Commands:
  init            Create a new Docker Extension based on a template.
  install         Install a Docker extension with the specified image
  ls              List installed Docker extensions
  rm              Remove a Docker extension
  share           Generate a link to share the extension.
  update          Remove and re-install a Docker extension
  validate        Validate an extension image or metadata file
  version         Print the client and server versions


```