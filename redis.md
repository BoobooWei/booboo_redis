# Redis


&#160; &#160; &#160; &#160;

## Redis入门


&#160; &#160; &#160; &#160;Redis是一个开源的（BSD许可），基于内存的存储系统，可以用作于缓存系统，数据库等，支持多种数据结构，支持主从，高可用等特性。由于Redis出色的性能表现与丰富的功能，经常被用于各种高效的数据检索环境或者缓冲环境中。

### Redis 特性


* 性能高
* 多种数据结构
* 持久化
* 功能丰富


### Redis 基本安装使用


#### 初始化环境

启动实验主机，演示主机为matsera0 ，你们需要按自己的主机名启动。

```shell
[kiosk@foundation0 ~]$ rht-vmctl  start mastera
Downloading virtual machine definition file for mastera.
######################################################################## 100.0%
Downloading virtual machine disk image db100-mastera-vda.qcow2
######################################################################## 100.0%
Creating virtual machine disk overlay for db100-mastera-vda.qcow2
Downloading virtual machine disk image db100-mastera-vdb.qcow2
######################################################################## 100.0%
Creating virtual machine disk overlay for db100-mastera-vdb.qcow2
Starting mastera.
```

登录到实验主机上并配置基本环境

* 登录并配置相关安装源

```shell

[kiosk@foundation0 ~]$ ssh root@mastera0
Warning: Permanently added 'mastera0' (ECDSA) to the list of known hosts.
root@mastera0's password:
Last login: Sun Aug 14 08:19:23 2016 from 172.25.0.250
[root@mastera0 ~]# yum install wget -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
rhel_dvd                                                                                                                               | 4.1 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package wget.x86_64 0:1.14-10.el7_0.1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================================
 Package                          Arch                               Version                                       Repository                            Size
==============================================================================================================================================================
Installing:
 wget                             x86_64                             1.14-10.el7_0.1                               rhel_dvd                             546 k

Transaction Summary
==============================================================================================================================================================
Install  1 Package

Total download size: 546 k
Installed size: 2.0 M
Downloading packages:
wget-1.14-10.el7_0.1.x86_64.rpm                                                                                                        | 546 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : wget-1.14-10.el7_0.1.x86_64                                                                                                                1/1
  Verifying  : wget-1.14-10.el7_0.1.x86_64                                                                                                                1/1

Installed:
  wget.x86_64 0:1.14-10.el7_0.1                                                                                                                               

Complete!
[root@mastera0 ~]# cd /etc/yum.repos.d/
[root@mastera0 yum.repos.d]# wget http://172.25.254.254/content/courses/db100/rhel7.2/materials/redis.repo
--2016-08-14 08:23:15--  http://172.25.254.254/content/courses/db100/rhel7.2/materials/redis.repo
Connecting to 172.25.254.254:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 108
Saving to: ‘redis.repo’

100%[====================================================================================================================>] 108         --.-K/s   in 0s      

2016-08-14 08:23:15 (24.7 MB/s) - ‘redis.repo’ saved [108/108]

[root@mastera0 yum.repos.d]# ls
redhat.repo  redis.repo  rhel_dvd.repo
[root@mastera0 yum.repos.d]# yum makecache
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
redis                                                                                                                                  | 2.9 kB  00:00:00     
rhel_dvd                                                                                                                               | 4.1 kB  00:00:00     
(1/3): redis/filelists_db                                                                                                              | 1.6 kB  00:00:00     
(2/3): redis/other_db                                                                                                                  | 2.4 kB  00:00:00     
(3/3): redis/primary_db                                                                                                                | 3.7 kB  00:00:00     
Metadata Cache Created
```

* 安装相关rpm包

```shell
[root@mastera0 yum.repos.d]# yum install redis -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Resolving Dependencies
--> Running transaction check
---> Package redis.x86_64 0:2.8.19-2.el7 will be installed
--> Processing Dependency: libjemalloc.so.1()(64bit) for package: redis-2.8.19-2.el7.x86_64
--> Running transaction check
---> Package jemalloc.x86_64 0:3.6.0-1.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================================
 Package                               Arch                                Version                                   Repository                          Size
==============================================================================================================================================================
Installing:
 redis                                 x86_64                              2.8.19-2.el7                              redis                              419 k
Installing for dependencies:
 jemalloc                              x86_64                              3.6.0-1.el7                               redis                              105 k

Transaction Summary
==============================================================================================================================================================
Install  1 Package (+1 Dependent package)

Total download size: 523 k
Installed size: 1.4 M
Downloading packages:
(1/2): jemalloc-3.6.0-1.el7.x86_64.rpm                                                                                                 | 105 kB  00:00:00     
(2/2): redis-2.8.19-2.el7.x86_64.rpm                                                                                                   | 419 kB  00:00:00     
--------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                         8.9 MB/s | 523 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : jemalloc-3.6.0-1.el7.x86_64                                                                                                                1/2
  Installing : redis-2.8.19-2.el7.x86_64                                                                                                                  2/2
  Verifying  : redis-2.8.19-2.el7.x86_64                                                                                                                  1/2
  Verifying  : jemalloc-3.6.0-1.el7.x86_64                                                                                                                2/2

Installed:
  redis.x86_64 0:2.8.19-2.el7                                                                                                                                 

Dependency Installed:
  jemalloc.x86_64 0:3.6.0-1.el7                                                                                                                               

Complete!

```

* 配置防火墙
* 配置selinux

redis默认使用6379端口，所以需要在防火墙中允许此端口，或者关闭防火墙

```shell
[root@mastera0 ~]# systemctl  stop firewalld
[root@mastera0 ~]# systemctl  disable firewalld
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
Removed symlink /etc/systemd/system/basic.target.wants/firewalld.service.
```

禁用selinux

```shell
[root@mastera0 ~]# setenforce 0
[root@mastera0 ~]# vi /etc/selinux/config
SELINUX=permissive
```


#### 包组成

redis包由以下文件组成

```shell
[root@mastera0 ~]# rpm -ql redis
/etc/logrotate.d/redis
/etc/redis-sentinel.conf
/etc/redis.conf
/etc/systemd/system/redis-sentinel.service.d
/etc/systemd/system/redis-sentinel.service.d/limit.conf
/etc/systemd/system/redis.service.d
/etc/systemd/system/redis.service.d/limit.conf
/usr/bin/redis-benchmark
/usr/bin/redis-check-aof
/usr/bin/redis-check-dump
/usr/bin/redis-cli
/usr/bin/redis-sentinel
/usr/bin/redis-server
/usr/bin/redis-shutdown
/usr/lib/systemd/system/redis-sentinel.service
/usr/lib/systemd/system/redis.service
/usr/lib/tmpfiles.d/redis.conf
/usr/share/doc/redis-2.8.19
/usr/share/doc/redis-2.8.19/00-RELEASENOTES
/usr/share/doc/redis-2.8.19/BUGS
/usr/share/doc/redis-2.8.19/CONTRIBUTING
/usr/share/doc/redis-2.8.19/MANIFESTO
/usr/share/doc/redis-2.8.19/README
/usr/share/licenses/redis-2.8.19
/usr/share/licenses/redis-2.8.19/COPYING
/var/lib/redis
/var/log/redis
/var/run/redis
```

其中主配置文件

> /etc/redis.conf

启动脚本

> /usr/lib/systemd/system/redis.service

相关命令

| 命令                        | 介绍          |
| :------------------------ | :---------- |
| /usr/bin/redis-benchmark  | 性能测试命令      |
| /usr/bin/redis-check-aof  | aof文件修复命令   |
| /usr/bin/redis-check-dump | rdb文件检查命令   |
| /usr/bin/redis-cli        | 命令行客户端      |
| /usr/bin/redis-server     | redis服务启动命令 |
| /usr/bin/redis-shutdown   | redis服务停止命令 |


启动redis

```shell
[root@mastera0 ~]# systemctl start redis
```
​

### Redis使用初步

登录redis

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379>
```

数据库选择

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> select 0
```

键值创建赋值语法

| 语法                              | 作用        |
| :------------------------------ | :-------- |
| set key value                   | 设定单个键值    |
| mset key1 value1 key2 value2 …. | 设定多个键值    |
| Append key value                | 对一个key追加值 |

Ex: 设定name键，值为mark

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> set  name  mark
OK
```
Ex: 设定name2键，值为jack , 追加赋值 tom

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> set name2 "jack"
OK
127.0.0.1:6379> get name2
"jack"
127.0.0.1:6379> append name2 " tom"
(integer) 8
127.0.0.1:6379> get name2
"jack tom"
127.0.0.1:6379>
```

 查询语法

| 语法                    | 作用         |
| :-------------------- | :--------- |
| keys [ keyname / * ] | 列出已经生效的key |
| get   keyname         | 查询某个key的值  |

Ex: 列出当前生效的key，并查询name的值
```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> keys  *
1) name
2) "name2"
127.0.0.1:6379> get name
"mark"
```

其他基础语法

| 语法                  | 作用                   |
| :------------------ | :------------------- |
| del keyname         | 删除一个key              |
| rename key newkey   | 重命名一个key             |
| renamenx key newkey | 重命名一个key (不会覆盖同名key) |
| exists key          | 判断一个键是否存在            |
| move key dbnumber   | 将一个key移动到其他库         |
| flushdb             | 清空当前库                |
| type keyname        | 查询一个键的数据类型           |

Ex：删除name健，并判断name是否还存在

```shell​
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> del name
(integer) 1
127.0.0.1:6379> exists name
(integer) 0
```

Ex：在０号库中创建name键，并将它移动到２号库

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> select 0
OK
127.0.0.1:6379> set name mark
OK
127.0.0.1:6379> get name
"mark"
127.0.0.1:6379> move name 2
(integer) 1
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> select 2
OK
127.0.0.1:6379[2]> get name
"mark"
```


Ex：清空当前库

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> flushdb
OK
```


## Redis 常见数据类型


### 字符串

语法：Set key value [ex] [px] [nx/xx]

| 参数              | 作用              |
| :-------------- | :-------------- |
| EX seconds      | 设置指定的到期时间，单位为秒。 |
| PX milliseconds | 设置指定到期时间，单位为毫秒。 |
| NX              | 设置键，如果它不存在。     |
| XX              | 设置键，如果它已经存在。    |
| -------------   | -------------   |

Ex : 如果name键不存在则设置键name，值为mark,生效时间为10秒

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> set name mark ex 10 nx
OK
127.0.0.1:6379[2]> get name
"mark"
127.0.0.1:6379[2]> get name
(nil)
```

相关操作命令

| 语法                               | 作用        |
| :------------------------------- | :-------- |
| mset key1 vaule1 key2 value2 ... | 同时设置多个key |
| get key                          | 取值        |
| mget key1 key2                   | 取多个key值   |
| append                           | 追加key值    |
| getset                           | 取得旧值并设置新值 |


redis并未提供整数型数据类型，但确有一些专用指令可以回复整数结果

| 语法                          | 作用                        |
| :-------------------------- | :------------------------ |
| incr key                    | 作用: 指定的key的值加1,并返回加1后的值   |
| incrby key number           | 指定的key的值加number,并返回值      |
| incrbyfloat key floatnumber | 指定的key的值加floatnumber,并返回值 |
| decr key                    | 指定的key的值减1,并返回减1后的值       |
| decrby key number           | 指定的key的值减number,并返回值      |


Ex:设置键number，初始值为10，做一次加1操作，一次加10操作，一次减1操作，一次减5操作，一次加0.5操作

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> set number 10
OK
127.0.0.1:6379> INCR number
(integer) 11
127.0.0.1:6379> INCRBY number 10
(integer) 21
127.0.0.1:6379> DECR number
(integer) 20
127.0.0.1:6379> DECRBY number 5
(integer) 15
127.0.0.1:6379> INCRBYFLOAT number 0.5
"15.5"
```


### 散列hash

Hash格式由键名，字段以及值组成，其功能类似关联数组。

![hash](pic/hash.jpg)

相关命令语法

| 语法                                       | 作用           |
| :--------------------------------------- | :----------- |
| hset key field value                     | 创建hash并定义一个域 |
| hmset key field1 value1 [field2 value2 field3 value3 ......fieldn valuen] | 创建hash并定义多个域 |
| hget key field                           | 取值(单个域)      |
| hmget key field1 field2 fieldN           | 取值(多个域)      |
| hgetall key                              | 取值(所有域)      |
| hdel key field                           | 删除一个域        |

Example：创建hash散列（user1）,定义此用户性别（Gender） 身高（180）体重　（75）　年龄　（28）　工作（sale）

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> hmset user1 Gender male height 180 weight 75 Age 28 job sale
OK
127.0.0.1:6379> hget user1 Gender
"male"
127.0.0.1:6379> hgetall user1
 1) "Gender"
 2) "male"
 3) "height"
 4) "180"
 5) "weight"
 6) "75"
 7) "Age"
 8) "28"
 9) "job"
10) "sale"
```


课后练习：使用hash来记录书籍相关信息（ID，书名，作者，发行日期，价格）



### 列表 list

列表用于存储一个有序字符串链表。我们可以在其头部(left)和尾部(right)添加新的元素。

![list](pic/list.jpg)

基本语法

| 语法                      | 作用                                       |
| :---------------------- | :--------------------------------------- |
| lpush key value [value] | 把值插入到链接头部                                |
| rpush key value [value] | 把值插入到链接尾部                                |
| lpop key                | 删除链表头部元素                                 |
| rpop key                | 删除链表尾部元素                                 |
| lrange key start  stop  | 返回链表中[start ,stop]中的元素                   |
| lrem key count value    | 删除若干（count）个value值，count为正数代表从头部开始删除，count为负数则从尾部开始删除 |
| ltrim key start stop    | 剪切链表,切[start,stop]一段,并把该段重新赋给key         |
| lindex key index        | 返回index索引上的值                             |
| llen key                | 返回链表长度                                   |
| linsert  key after      | before search value                      |


Ex:创建列表route，在列表尾部添加r7值，取出列表内容，取出列表内第3个元素

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> RPUSH route R1 R5 R6
(integer) 3
127.0.0.1:6379> LRANGE route 0 -1
1) "R1"
2) "R5"
3) "R6"
127.0.0.1:6379> RPUSH route R7
(integer) 4
127.0.0.1:6379> LRANGE route 0 -1
1) "R1"
2) "R5"
3) "R6"
4) "R7
127.0.0.1:6379> LINDEX route 2
"R6"
```

课后练习：使用list来存储每本书的章节信息


### 集合


由元素组成的总体叫做集合，也简称集。每个元素不同，且无序。

![set.jpg](pic/set.jpg)

基本语法

| 语法                              | 作用                                |
| :------------------------------ | :-------------------------------- |
| sadd key  value1 value2         | 往集合key中增加元素                       |
| srem value1 value2              | 删除集合中集为 value1 value2的元素          |
| spop key                        | 返回并删除集合中key中1个随机元素                |
| srandmember key                 | 返回集合key中,随机的1个元素                  |
| smembers key                    | 返回集中中所有的元素                        |
| sismember key  value            | 判断value是否在key集合中                  |
| scard key                       | 返回集合中元素的个数                        |
| smove source dest value         | 将value从source集合移动到dest集合          |
| sinter  key1 key2 key3          | 求出key1 key2 key3 三个集合中的交集         |
| sunion key1 key2.. Keyn         | 求出key1 key2 keyn的并集,并返回           |
| sinterstore dest key1 key2 key3 | 求出key1 key2 key3 三个集合中的交集,并赋给dest |
| sdiff key1 key2 key3            | 求出key1与key2 key3的差集               |

Ex:	创建集合word1,word2。返回集合wrod2中所有内容，求word1与word2的并集，交集，差集

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> SADD word1 a d c f k e
(integer) 6
127.0.0.1:6379> SADD word2 a d p v m e  r t
(integer) 8
127.0.0.1:6379> SMEMBERS word1
1) "f"
2) "c"
3) "d"
4) "a"
5) "k"
6) "e"
127.0.0.1:6379> SUNION word1 word2
 1) "a"
 2) "d"
 3) "m"
  4) "f"
  5) "p"
  6) "k"
  7) "r"
  8) "e"
  9) "v"
10) "t"
11) "c"
127.0.0.1:6379> SINTER word1 word2
1) "d"
2) "a"
3) "e"
127.0.0.1:6379> SDIFF word1 word2
1) "k"
2) "c"
3) "f"
```
课后练习：使用集合来定义书籍分类信息


### 有序集合

&#160; &#160; &#160; &#160;与集合的区别只是有序而已，与列表有很像，但与列表有几个不同之处。列表操作头尾两端数据性能比特别高效，有序集合不是，列表不能简单的实现某个元素的位置调整，有序集合可以，列表内的元素可以重复，有序集合不可，有序集合比列表更消耗内存。

基本语法

| 语法                                      | 作用                                       |
| :-------------------------------------- | :--------------------------------------- |
| zadd key score1 value1 score2 value2 .. | 添加元素                                     |
| zrem key value1 value2 ..               | 删除集合中的元素                                 |
| zremrangebyscore key min max            | 按照socre来删除元素,删除score在[min,max]之间的        |
| zremrangebyrank key start end           | 按排名删除元素,删除名次在[start,end]之间的              |
| zrank key value1                        | 查询value1的排名                              |
| zrevrank key value1                     | 查询 value1的排名(降续 0名开始)                    |
| ZRANGE key start stop [WITHSCORES]      | 把集合排序后,返回名次[start,stop]的元素 默认是升续排列 Withscores 是把score也打印出来 |
| zrevrange key start stop                | 把集合降序排列,取名字[start,stop]之间的元素             |
| zcard key                               | 返回元素个数                                   |
| zcount key min max                      | 返回[min,max] 区间内元素的数量                     |
​
Ex:创建有序集合ranking，查看集合，查询lisi的排名

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> ZADD ranking 45 zhangsan 60 lisi 85 laowang  70 xiaoming
(integer) 4
127.0.0.1:6379> ZRANGE ranking 0 -1
1) "zhangsan"
2) "lisi"
3) "xiaoming"
4) "laowang
127.0.0.1:6379> ZRANK ranking lisi
(integer) 1
127.0.0.1:6379> zrange ranking 0 2
1) "zhangsan"
2) "lisi"
3) "xiaoming"
```

课后练习：使用有序集合来定义top10排行榜


&#160; &#160; &#160; &#160;还有更多其他的数据类型这里就不一一讲解了，可以自行参考官方帮助文档。


## Redis 事务

在Redis中，事务是一组命令的集合。

语法格式:

	Multi
	…..
	…..
	Exec

&#160; &#160; &#160; &#160;Redis的事务功能非常简单，并未像其他数据库（mysql,oracle）那样可以实现事务性ACID的特性。
​
&#160; &#160; &#160; &#160;以原子性(Atomicity )为例，如果multi~exec之间有明显的语法错误，则所有语句失效，如果是操作的数据出错，则会忽略出错语句，执行其他正确语句。

例-error1：

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set name1 zhangsan
QUEUED
127.0.0.1:6379> set name2
(error) ERR wrong number of arguments for 'set' command
127.0.0.1:6379> set name3 lisi
QUEUED
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get name1
```

&#160; &#160; &#160; &#160;上述例子中可以看到因为语法错误而导致所有语句全部未执行。


例-error2：

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> RPUSH testlist aa bb cc dd
QUEUED
127.0.0.1:6379> LRANGE testlist 0 -1
QUEUED
127.0.0.1:6379> zadd testlist 10 ee
QUEUED
127.0.0.1:6379> RPUSH testlist ff
QUEUED
127.0.0.1:6379> exec
1) (integer) 4
2) 1) "aa"
 	2) "bb"
	3) "cc"
	4) "dd"
3) (error) WRONGTYPE Operation against a key holding the wrong kind of value
4) (integer) 5
127.0.0.1:6379> LRANGE testlist 0 -1
1) "aa"
2) "bb"
3) "cc"
4) "dd"
5) "ff"
```

&#160; &#160; &#160; &#160;可以看到zadd语句是有问题的，执行过程中zadd被忽略，其他语句仍然执行了。

&#160; &#160; &#160; &#160;另一个与事务处理相关的命令watch. 此命令可以用于监控某些键值，以决定是否运行事务。

&#160; &#160; &#160; &#160;例-watch

终端1：

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> set key1 100
OK
127.0.0.1:6379> get key1
"100"
127.0.0.1:6379> watch key1
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> INCR key1
QUEUED
```

终端2：

```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> set key1 101
OK
```

终端1继续：
```shell
[root@mastera0 ~]# redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> exec
(nil)
```

&#160; &#160; &#160; &#160;可以看到所有事务都没执行。那是因为在事务定义过程中key1发生了改变，并且使用了watch命令对key1做了监控。Watch命令发现自己监控的键值发生改变，那么就会取消事务执行。

&#160; &#160; &#160; &#160;

## Redis 持久化

&#160; &#160; &#160; &#160;Redis之所以拥有非常高的性能，很大一个原因是只使用内存进行读写，所以一但宕机或重启导致内存数据丢失，那么Redis数据库也会丢失。为了解决这个问题，Redis需要能够将数据保存到硬盘上，这个过程称之为持久化。

&#160; &#160; &#160; &#160;Redis持久化有两种方案，分别为RDB方式与AOF方式。

### RDB持久化：

&#160; &#160; &#160; &#160;RDB的实现原理非常简单，根据一定的规则周期化的将内存中的Redis的数据同步到硬盘上。整个过程会使用类似快照的方法来实现。简单的讲，父进程先fork出一个子进程，之后父进程继续处理用户请求，子进程创建临时文件执行数据同步。为了保证数据一致性，会启用COW方式进行读写。当数据同步完成后再用临时文件覆盖真正的RDB文件。

&#160; &#160; &#160; &#160;同步规则主要由配置文件中的save参数定义。
​
	save 900 1
	save 300 10
	save 60 10000

&#160; &#160; &#160; &#160;以上这些参数的含义为：
&#160; &#160; &#160; &#160;900秒内有10个以上的键被更改则执行RDB同步，300秒内有10个键发生更改则同步，60秒内有10000个键发生更改则同步。三者关系为“或”，只要满足任何一个条件都会执行同步操作。

&#160; &#160; &#160; &#160;除了以上参数以外，还有些指令与事件也会自动触发同步，比如save,bgsave,flushall,主从同步事件等等。

&#160; &#160; &#160; &#160;RDB方式有一个比较严重的问题，如果在一次同步完成之后，连续15分钟内只做了2次修改，那么最终要到15分钟的时候才会同步，对应的对数为“save 900 1”。那如果在15分钟服务器宕机或者重启了，那么就有可能丢失这2次操作，所以RDB方式的持久化并不是非常完整。

Ex:配置RDB持久化并测试

```shell
[root@mastera0 ~]# vi /etc/redis.conf
save 900 1
save 300 10
save 60 10000
dbfilename dump.rdb
dir /var/lib/redis/

[root@mastera0 ~]# systemctl  restart redis
[root@mastera0 ~]# redis-cli  -h 127.0.0.1
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> set name aa
OK
127.0.0.1:6379> exit

等待15分钟
[root@mastera0 ~]# systemctl  restart redis
[root@mastera0 ~]# redis-cli  -h 127.0.0.1
127.0.0.1:6379> get name
"aa"

```

### AOF持久化

&#160; &#160; &#160; &#160;相对于RDB方式而言，AOF对数据完整性就做的更好了。AOF会将用户执行过的每一条指令都记录到硬盘上。如果出现意外中断，那么可以通过重演指令来还原数据库。由于经常要将指令同步到硬盘上，所以会对性能造成一定影响。

&#160; &#160; &#160; &#160;相关参数：

​
	appendonly yes

	auto-aof-rewrite-percentage 100

	auto-aof-rewrite-min-size 64mb

&#160; &#160; &#160; &#160;以上两个参数用于重写AOF文件，以便自动去除重复指令，减小文件体积。

	appendfsync [always|everysec|no]

&#160; &#160; &#160; &#160;此参数用于定义同步的频率，always代表每个操作都立刻同步，everysec代表1秒同步1次，no代表按操作系统默认的同步方式（Linux默认内存中修改过的文件会以30秒为周期同步到硬盘）。


Ex：配置AOF持久化并测试

```shell
[root@mastera0 ~]# vi /etc/redis.conf
appendonly yes
appendfsync everysec
[root@mastera0 ~]# systemctl  restart redis
[root@mastera0 ~]# redis-cli  -h 127.0.0.1
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> set name aa
OK
127.0.0.1:6379> exit
[root@mastera0 ~]# systemctl  restart redis
[root@mastera0 ~]# redis-cli  -h 127.0.0.1
127.0.0.1:6379> get name
"aa"
```


&#160; &#160; &#160; &#160;Redis允许RDB与AOF是可以同时打开。

## Redis 主从

&#160; &#160; &#160; &#160;Redis可以通过持久化方式，将数据保存在硬盘上，防止数据丢失，但这样仍然存在一些问题，比如单点故障，当redis主机发生宕机，会严重影响到服务。比较常见的做法是使用多台主机来运行Redis，并且通过数据同步实现其一致性。我们将此环境称之为Redis主从。

&#160; &#160; &#160; &#160;Redis主从分为master（主）与slave（从）。一般情况下，主服务器可以读写，从机只读。主服务器会将所有写操作同步给slave。一个主服务器可以拥有多个slave。一个slave只能有一个master。

 &#160; &#160; &#160; &#160;从服务器启用时，会向主服务器发送sync指令。此时，主服务器会立刻执行RDB快照备份，并将备份过程中收到的命令缓冲下来。RDB备份完成后，将备份文件与缓冲命令一起传输给从机，从机加载备份文件并执行缓冲命令，实现数据一致。这个过程称为复制初始化。自此以后主服务器执行的所有命令都将以异步方式传输到从机。

&#160; &#160; &#160; &#160;相关参数：

	slaveof masterip msaterport

	slave-read-only

&#160; &#160; &#160; &#160;主从同步的作用不单单是为了提高可用性，同时也可以通过负载均衡和读写分离来提升性能。另外有一个和性能相关的建议，如果你需要搭建一个主从环境，那你可以考虑在主服务器上停用RDB备份，只在从机上启用即可。RDB备份是一种比较消耗性能的操作，这样我们可以相对节约主服务器性能消耗。不过就算你关闭了RDB备份，但是在执行复制初始化过程中，乃会创建一个RDB文件，此时你可以通过以下参数开启redis无硬盘复制模式。此模式一但开启，主服务器会将RDB备份文件直接传输给从机，而不会保存到硬盘上。

	repl-diskless-sync yes

Ex：配置主从（master:mastera0 ip:172.25.0.10 , slave:slave0 ip:172.25.0.12）

登录master redis : mastera0，将监听ＩＰ改为网卡ＩＰ，以便与slave通信

```shell
[root@mastera0 ~]# vi /etc/redis.conf
bind 172.25.0.11 127.0.0.1
[root@mastera0 ~]# systemctl  restart redis
```

物理机上启动slavea虚拟机　并登录

```shell
[kiosk@foundation0 materials]$ rht-vmctl  start slavea
Downloading virtual machine definition file for slavea.
######################################################################## 100.0%
Downloading virtual machine disk image db100-slavea-vda.qcow2
######################################################################## 100.0%
Creating virtual machine disk overlay for db100-slavea-vda.qcow2
Downloading virtual machine disk image db100-slavea-vdb.qcow2
######################################################################## 100.0%
Creating virtual machine disk overlay for db100-slavea-vdb.qcow2
Starting slavea.

[kiosk@foundation0 materials]$ ssh root@slavea0
root@slavea0's password:
Last login: Sun Apr 24 14:43:13 2016
[root@slavea0 ~]#
```

初始化slave虚拟机环境(安装源，防火墙，selinux)

```shell
[root@slavea0 ~]# yum install wget -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
rhel_dvd                                                                                                                               | 4.1 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package wget.x86_64 0:1.14-10.el7_0.1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================================
 Package                          Arch                               Version                                       Repository                            Size
==============================================================================================================================================================
Installing:
 wget                             x86_64                             1.14-10.el7_0.1                               rhel_dvd                             546 k

Transaction Summary
==============================================================================================================================================================
Install  1 Package

Total download size: 546 k
Installed size: 2.0 M
Downloading packages:
wget-1.14-10.el7_0.1.x86_64.rpm                                                                                                        | 546 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : wget-1.14-10.el7_0.1.x86_64                                                                                                                1/1
  Verifying  : wget-1.14-10.el7_0.1.x86_64                                                                                                                1/1

Installed:
  wget.x86_64 0:1.14-10.el7_0.1                                                                                                                               

Complete!

[root@slavea0 ~]# cd /etc/yum.repos.d/
[root@slavea0 yum.repos.d]# wget http://172.25.254.254/content/courses/db100/rhel7.2/materials/redis.repo
--2016-08-14 08:45:03--  http://172.25.254.254/content/courses/db100/rhel7.2/materials/redis.repo
Connecting to 172.25.254.254:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 108
Saving to: ‘redis.repo’

100%[====================================================================================================================>] 108         --.-K/s   in 0s      

2016-08-14 08:45:03 (18.2 MB/s) - ‘redis.repo’ saved [108/108]

[root@slavea0 yum.repos.d]# yum install redis -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
redis                                                                                                                                  | 2.9 kB  00:00:00     
redis/primary_db                                                                                                                       | 3.7 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package redis.x86_64 0:2.8.19-2.el7 will be installed
--> Processing Dependency: libjemalloc.so.1()(64bit) for package: redis-2.8.19-2.el7.x86_64
--> Running transaction check
---> Package jemalloc.x86_64 0:3.6.0-1.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================================
 Package                               Arch                                Version                                   Repository                          Size
==============================================================================================================================================================
Installing:
 redis                                 x86_64                              2.8.19-2.el7                              redis                              419 k
Installing for dependencies:
 jemalloc                              x86_64                              3.6.0-1.el7                               redis                              105 k

Transaction Summary
==============================================================================================================================================================
Install  1 Package (+1 Dependent package)

Total download size: 523 k
Installed size: 1.4 M
Downloading packages:
(1/2): jemalloc-3.6.0-1.el7.x86_64.rpm                                                                                                 | 105 kB  00:00:00     
(2/2): redis-2.8.19-2.el7.x86_64.rpm                                                                                                   | 419 kB  00:00:00     
--------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                         9.0 MB/s | 523 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : jemalloc-3.6.0-1.el7.x86_64                                                                                                                1/2
  Installing : redis-2.8.19-2.el7.x86_64                                                                                                                  2/2
  Verifying  : redis-2.8.19-2.el7.x86_64                                                                                                                  1/2
  Verifying  : jemalloc-3.6.0-1.el7.x86_64                                                                                                                2/2

Installed:
  redis.x86_64 0:2.8.19-2.el7                                                                                                                                 

Dependency Installed:
  jemalloc.x86_64 0:3.6.0-1.el7                                                                                                                               

Complete!

[root@slavea0 yum.repos.d]# systemctl  stop firewalld
[root@slavea0 yum.repos.d]# systemctl  disable firewalld
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
Removed symlink /etc/systemd/system/basic.target.wants/firewalld.service.

[root@mastera0 ~]# setenforce 0
[root@mastera0 ~]# vi /etc/selinux/config
SELINUX=permissive
```


配置slave redis: slavea0
```shell
[root@slavea0 ~]# vi /etc/redis.conf
bind 172.25.0.13 127.0.0.1
slaveof 172.25.0.11 6379
[root@slavea0 ~]# systemctl  restart redis
```

切换至master redis : mastera0 ，创建测试数据test
```shell
[root@mastera0 ~]# redis-cli  -h 127.0.0.1
127.0.0.1:6379> set test replication
OK
127.0.0.1:6379> get test
"replication"
```

切换至slave redis : slavea0 ，验证主从结果
```shell
[root@slavea0 ~]# redis-cli  -h 127.0.0.1
127.0.0.1:6379> get test
"replication"
```
