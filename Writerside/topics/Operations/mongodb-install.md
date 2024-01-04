# mongo安装

## mongo副本集_郭秀

1. 搭建环境  3台mongodb
2. 修改配置文件mongodb.conf加入参数`replSet=reptest`
3. 启动一台mongodb`/usr/local/mongodb/mongo/bin/mongod -f /usr/local/mongodb/mongo/bin/mongodb.conf --replSet reptest`
4. 启动mongodb sehll(数据操作界面) 在bin 目录下  `./mongo --port 27017`
5. 切换到admin 数据库 `use admin`
6. 配置副本集的参数
```javascript
config = { _id:"reptest",
         members:[  {_id:0,host:"192.168.198.128:27017"},         				
                    {_id:1,host:"192.168.198.129:27017"},
                    {_id:2,host:"192.168.198.130:27017"}
        ]
}
```

7. 加载副本集配置文件 `rs.initiate(config)`
8. 查看副本集状态 `rs.status()`

正常情况下可以看到192.168.198.128会是主服务器，显示PRIMARY,如果是，就直接进行以下操作，如果不是，就切换到PRIMARY上进行以下操作（换到另一个mongo）；


## 用户认证

1. 增加用户 `db.createUser({"user":"admin","pwd":"admin","roles":["root"]})`
2.更改用户验证方式
```
var schema=db.system.version.findOne({"_id":"authSchema"})  
schema.currentVersion=3  
db.system.version.save(schema)
```
（8）、删除用户： db.dropUser("admin")

3. 重新建立用户(系统中和上边建立的用户验证方式不一样)`db.createUser({"user":"admin","pwd":"admin","roles":["root"]})`
4. 关闭三个mongodb `db.shutDownServer()或者kill命令 (建议不使用)`
5. 在57017的数据库的data目录中建立keyFile文件
```shell
cd /usr/local/mongodb/mongo/data  
openssl  rand  –base64 753  >  keyFile
```

6. 给keyFile文件设置600权限（必须设置600权限） `chmod 600 keyFile`
7. 把这个keyFile文件上传到另外两台机上mongodb的data目录中
```shell
scp  keyFile root@192.168.198.129:/usr/local/mongodb/mongo/data
scp  keyFile root@192.168.198.130:/usr/local/mongodb/mongo/data
```

8. 在mongodb.conf文件中加入keyFile,例如192.168.198.128 `keyFile=/usr/local/mongodb/mongo/data/keyFile`
9. 重新启动mongodb，
```shell
/usr/local/mongodb/mongo/bin/mongod –f 		        							
/usr/local/mongodb/mongo/bin/mongodb.conf --replSet reptest  --auth

```

10. 在priority中设置副本集成员的优先级，给128设置最高优先级，优先级默认都是1
```
config=rs.conf()
config.members[0].priority=2
rs.reconfig(config)
```

