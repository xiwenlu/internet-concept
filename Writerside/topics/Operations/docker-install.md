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

## elasticsearch
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
### spark安装
```Shell
docker run -it --name my-spark -p 4040:4040 apache/spark /opt/spark/bin/spark-shell
```

## mq安装
### rabbitmq安装

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


### activemq安装
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
