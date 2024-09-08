# 基础定义

在内存层面实现的key-value数据库（关系型数据库），查找更为迅速，用于热点资讯的查询、秒杀等实现。一般用于Linux。



## SQL和NoSQL的区别

|          | SQL                                                     | NoSQL                                                        |
| -------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| 数据结构 | 结构化                                                  | 非结构化                                                     |
| 数据关联 | 关联的                                                  | 无关联的                                                     |
| 查询方式 | SQL查询                                                 | 非SQL                                                        |
| 事务特性 | ACID                                                    | BASE                                                         |
| 存储方式 | **磁盘**                                                | **内存**                                                     |
| 扩展性   | **垂直**                                                | **水平**                                                     |
| 使用场景 | 1) 数据结构固定 2) 相关业务对数据安全性、一致性要求较高 | 1) 数据结构不固定 2) 对一致性、安全性要求高 3) 对性能要求较高 |



## Redis的特性

- 键值（key-value）型，value支持多种不同数据结构，功能丰富
- 单线程，每个命令具备原子性
- 低延迟，速度快（基于内存、IO多路复用、良好的编码）
- 支持数据持久化
- 支持主从集群、分片集群
- 支持多语言客户端



## Redis的控制使用



### 按照指定配置启动

在Redis的目录中修改配置文件：

```properties
# 允许访问的地址，默认是127.0.0.1，会导致只能在本地访问。修改为0.0.0.0则可以在任意IP访问，生产环境不要设置为0.0.0.0
bind 0.0.0.0
# 守护进程，修改为yes后即可后台运行
daemonize yes 
# 密码，设置后访问Redis必须输入密码
requirepass 123321
# 监听的端口
port 6379
# 工作目录，默认是当前目录，也就是运行redis-server时的命令，日志、持久化等文件会保存在这个目录
dir .
# 数据库数量，设置为1，代表只使用1个库，默认有16个库，编号0~15
databases 1
# 设置redis能够使用的最大内存
maxmemory 512mb
# 日志文件，默认为空，不记录日志，可以指定日志文件名
logfile "redis.log"
```



#### 手动启动Redis

```sh
# 进入redis安装目录
cd /usr/local/src/redis-6.2.6
# 启动
redis-server redis.conf
```



#### 停止Redis服务

```sh
# 利用redis-cli来执行 shutdown 命令，即可停止 Redis 服务，
# 因为之前配置了密码，因此需要通过 -u 来指定密码
redis-cli -u 123321 shutdown
```



### 配置开机自启动

新建系统服务文件

```sh
vi /etc/systemd/system/redis.service
```

配置文件如下

```properties
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /usr/local/src/redis-6.2.6/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

随后重载系统服务

```sh
systemctl daemon-reload
```

使用以下命令启停Redis

```sh
# 启动
systemctl start redis
# 停止
systemctl stop redis
# 重启
systemctl restart redis
# 查看状态
systemctl status redis
```

配置开机自启动

```sh
systemctl enable redis
```



### 图形化桌面客户端

该仓库为Windows环境下的图形化桌面客户端：https://github.com/lework/RedisDesktopManager-Windows/releases

配置连接即可



## 数据结构

- 字符串(string):普通字符串，Redis中最简单的数据类型。

- 哈希(hash):也叫散列，类似于Java中的HashMap结构。

- 列表(list):按照插入顺序排序，可以有重复元素，类似于Java中的LinkedList。
- 集合(set):无序集合，没有重复元素，类似于Java中的HashSet。
- 有序集合(sorted set / zset):集合中每个元素关联一个分数(score)，根据分数升序排序，没有重复元素。



# Redis命令



## 字符串类型常用命令

| 命令                    | 作用                                            |
| ----------------------- | ----------------------------------------------- |
| set key value           | 设置指定key的值                                 |
| get key                 | 获取指定key的值                                 |
| setex key seconds value | 设置指定key的值，并将key的过期时间设为seconds秒 |
| setnx key value         | 只有在key不存在时设置key的值                    |



## 哈希类型常用命令

是一个string类型的field和value的映射表，哈希特别适合用于存储对象。

| 命令                 | 作用                                  |
| -------------------- | ------------------------------------- |
| hset key field value | 将哈希表key中的字段field的值设为value |
| hget key field       | 获取存储在哈希表中指定字段的值        |
| hdel key field       | 删除存储在哈希表中的指定字段          |
| hkeys key            | 获取哈希表中所有的字段                |
| hvals key            | 获取哈希表中所有值                    |



## 列表类型常用命令

按照插入顺序排序。

| 命令                      | 作用                         |
| ------------------------- | ---------------------------- |
| LPUSH key value1 [value2] | 将一个或多个值插入到列表头部 |
| LRANGE key start stop     | 获取列表指定范围内的元素     |
| RPOP key                  | 移除并获取列表最后一个元素   |
| LLEN key                  | 获取列表长度                 |



## Set类型常用命令

集合成员是唯一的，集合中不能出现重复的数据。

| 命令                       | 作用                     |
| -------------------------- | ------------------------ |
| SADD key member1 [member2] | 向集合添加一个或多个成员 |
| SMEMBERS key               | 返回集合中的所有成员     |
| SCARD key                  | 获取集合的成员数         |
| SINTER key1 [key2]         | 返回给定所有集合的交集   |
| SUNION key1 [key2]         | 返回所有给定集合的并集   |
| SREM key member1 [member2] | 删除集合中一个或多个成员 |



## 有序集合类型常用命令

有序集合是string类型元素的集合，且不允许有重复成员。每个元素都会关联一个double类型的分数。

| 命令                                     | 作用                                        |
| ---------------------------------------- | ------------------------------------------- |
| ZADD key score1 member1 [score2 member2] | 向有序集合添加一个或多个成员                |
| ZRANGE key start stop [WITHSCORES]       | 通过索引区间返回有序集合中指定区间内的成员  |
| ZINCRBY key increment member             | 有序集合中对指定成员的分数加上增量increment |
| ZREM key member [member ...]             | 移除有序集合中的一个或多个成员              |



## 通用命令

| 命令               | 作用                                               |
| ------------------ | -------------------------------------------------- |
| KEYS pattern       | 查找所有符合给定模式的key                          |
| EXISTS key         | 检查给定key是否存在                                |
| TYPE key           | 返回key所储存的值的类型                            |
| DEL key            | 该命令用于在key存在时删除key                       |
| EXPIRE key time(s) | 给一个key设置有效期，有效期到期时该key会被自动删除 |
| TTL                | 查看一个KEY的剩余有效期                            |

**注意**：尽量避免在生产环境下使用`KEYS`命令查询，可能会造成堵塞。



### Redis的Java端

常用的有Jedis、Lettuce、Spring Data Redis。



### Spring Data Redis使用方式

操作步骤：

1. 导入Spring Data Redis的maven坐标
2. 配置redis数据源
3. 编写配置类，创建RedisTemplate对象
4. 通过RedisTemplate对象操作Redis



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```





### Spring Cache

一个用于操控内存数据库的框架，实现了基于注解的缓存功能，只需要添加简单的注解就能实现缓存功能。

Spring Cache提供了一层抽象，底层可以切换不同的缓存实现，比如：

- EHCache
- Caffeine
- Redis



常用注解如下

| 注解           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| @EnableCaching | 开启缓存注解功能，通常加在启动类上                           |
| @Cacheable     | 在方法执行前先查询缓存中是否有数据，如果有数据，则直接返回缓存数据；如果没有缓存数据，调用方法并将方法返回值放到缓存中 |
| @CachePut      | 将方法的返回值放到缓存中                                     |
| @CacheEvict    | 将一条或多条数据从缓存中删除                                 |

