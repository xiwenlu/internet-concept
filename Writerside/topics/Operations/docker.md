# docker

## 什么是Docker Extensions？

Docker Extensions是一个允许开发人员在Docker Desktop中构建新功能、扩展现有功能并发现和集成其他工具的扩展系统。它通过使用第三方工具来帮助开发人员扩展Docker的功能，从而提高工作效率和工作流的顺畅性。

### Disk Usage

通过从Docker Desktop中删除未使用的对象来优化磁盘空间。

### Tailscale

Tailscale使您能够安全地连接到Docker容器，而不会将它们暴露在公共互联网上。

### Logs Explorer

在一个位置查看所有容器日志，以便更快地进行调试和故障排除。

### Slim.AI

Slim使您能够更快地创建安全容器。深入研究你的图像结构。知道你的容器里有什么。

## docker命令

```Shell

# 查看容器的ip
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id

# 查看日志
docker logs -f --tail=100 921

# 进入容器
docker exec -it 775c7c9ee1e1 /bin/bash  

# 从容器复制到宿主机
docker cp redis:/data/appendonly.aof /home/docker/redis/conf

# 从宿主机复制到容器
docker cp /home/docker/redis/conf/appendonly.aof redis:/data

# 查看docker包含什么命令
docker help

# 查看docker extension 扩展包含什么命令
docker extension help

```

## Docker run命令常用参数

|      参数       |              描述              |
|:-------------:|:----------------------------:|
| -d, --detach  |           在后台运行容器            |
|    --name     |          为容器指定一个名称           |
|  --hostname   |           设置容器的主机名           |
|     --rm      |         容器退出后自动删除容器          |
| -v, --volume  |     给容器挂载存储卷，挂载到容器的某个目录      |
|   --network   |          指定容器的网络模式           |
|   -e, --env   |     指定环境变量，容器中可以使用该环境变量      |
| -p, --publish |          指定容器暴露的端口           |
|   --restart   |          设置容器的重启策略           |
| --privileged  |          给容器赋予额外的权限          |
|   --memory    |          指定容器的内存上限           |
|   --cpuset    | 设置容器可以使用哪些CPU，此参数可以用来容器独占CPU |
| --entrypoint  |         覆盖image的入口点          |
|     -itd      |     后台模式运行容器，并打开伪终端与容器交互     |

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

## 安装mq

### 安装rabbitmq

```Shell
# 启动rabbitmq，-v 可选
docker run --hostname my-rabbit \
           --name rabbit \
           -p 15672:15672 \
           -p 5672:5672 \
           -v rabbitmq_data:/var/lib/rabbitmq \
           rabbitmq:latest

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

访问：[http://localhost:15672](http://localhost:15672)，输入账号guest和密码guest即可登录。

### 安装activemq

```Shell
docker run --name=activemq \
            -itd \
            -p 8161:8161 \
            -p 61616:61616 \
            -e ACTIVEMQ_ADMIN_LOGIN=admin \
            -e ACTIVEMQ_ADMIN_PASSWORD=123456 \
            --restart=always \
            webcenter/activemq:latest
```

## 安装mysql
```Shell
docker run --name my-mysql -e MYSQL_ROOT_PASSWORD=12345678 -d mysql
```