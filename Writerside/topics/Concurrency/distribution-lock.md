# 分布式锁

## 什么是分布式事务？

分布式事务就是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的不同节点之上。简单的说，就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。

简单来讲，当数据库单表一年产生的数据超过1000W，那么就要考虑分库分表，简单的说就是原来的一个数据库变成了多个数据库。这时候，如果一个操作既访问01库，又访问02库，而且要保证数据的一致性，那么就要用到分布式事务。

### 分布式事务产生的原因

1. 数据库分库分表：当数据库单表一年产生的数据超过1000W，那么就要考虑分库分表，具体分库分表的原理在此不做解释，以后有空详细说，简单的说就是原来的一个数据库变成了多个数据库。这时候，如果一个操作既访问01库，又访问02库，而且要保证数据的一致性，那么就要用到分布式事务。
2. 应用SOA化：所谓的SOA化，就是业务的服务化。比如原来单机支撑了整个电商网站，现在对整个网站进行拆解，分离出了订单中心、用户中心、库存中心。对于订单中心，有专门的数据库存储订单信息，用户中心也有专门的数据库存储用户信息，库存中心也会有专门的数据库存储库存信息。这时候如果要同时对订单和库存进行操作，那么就会涉及到订单数据库和库存数据库，为了保证数据一致性，就需要用到分布式事务。

## 什么是分布式锁？

为了保证一个方法在高并发情况下的同一时间只能被同一个线程执行，在传统单体应用单机部署的情况下，可以使用Java并发处理相关的API(
如Reentrant Lock或synchronized)进行互斥控制。

但是，随着业务发展的需要，原单体单机部署的系统被演化成分布式系统后，由于分布式系统多线程、多进程并且分布在不同机器上，这将使原单机部署情况下的并发控制锁策略失效，为了解决这个问题就需要一种跨JVM的互斥机制来控制共享资源的访问，这就是分布式锁要解决的问题。

## 分布式锁应该具备哪些条件

1. 获取锁和释放锁的性能要好
2. 判断是否获得锁必须是原子性的，否则可能导致多个请求都获取到锁
3. 网络中断或宕机无法释放锁时，锁必须被清楚，不然会发生死锁
4. 可重入一个线程中可以多次获取同一把锁，比如一个线程在执行一个带锁的方法，该方法中又调用了另一个需要相同锁的方法，则该线程可以直接执行调用的方法，而无需重新获得锁；
5. 阻塞锁和非阻塞锁，阻塞锁即没有获取到锁，则继续等待获取锁；非阻塞锁即没有获取到锁后，不继续等待，直接返回锁失败。

## 分布式锁实现方式

1. 数据库锁：基于MySQL锁表，采用乐观锁增加版本号
2. 缓存锁：基于setnx、expire两个命令来实现，RedLock算法


