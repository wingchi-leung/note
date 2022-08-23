# redis

 运行：

redis-server.exe redis.windows.conf 

cd usr/local/redis 

redis-server redis.conf



Remote Dictionary Server （远程字典服务） 

## 数据结构

### string 

 Redis的字符串就是一个由字节组成的序列。二进制安全。

可以存储三种类型：

- 字节串byte string
- 整数
- 浮点数

| 命令   | 简述                   | 使用              |
| ------ | ---------------------- | ----------------- |
| GET    | 获取存储在给定键中的值 | GET name          |
| SET    | 设置存储在给定键中的值 | SET name value    |
| DEL    | 删除存储在给定键中的值 | DEL name          |
| INCR   | 将键存储的值加1        | INCR key          |
| DECR   | 将键存储的值减1        | DECR key          |
| INCRBY | 将键存储的值加上整数   | INCRBY key amount |
| DECRBY | 将键存储的值减去整数   | DECRBY key amount |



Redis的字符串是使用SDS（simple dynamic string) 封装，sds源码如下：

```c
struct sdshdr{
 unsigned int len ; //buf的长度 
 unsigned int free ; //标记buf中没使用的元素个数 
 char buf[] ;  // 存放元素的数组 
}
```





应用场景：共享session、分布式锁、限流

- 计数器：使用redis作为系统的实时计数器，可以快速实现计数和查询的功能

- 共享session：用户重新刷新一次界面，可能需要访问一下数据进行重新登录，或者访问页面cookie； 

  ​	这两种做法有一定弊端：1、每次都要重新登录效率低下 2、cookie保存在客户端有安全隐患。  

  ​	这时可以利用redis将用户的session集中管理，在这种模式只需要保证redis的高可用，每次用户session的更新和获取都可以快速完成，大大提高效率。



### list

链表。 用来存储多个有序的字符串。 一个列表最多可以存储 2^32-1个元素，用双端链表实现的

| 命令     | 用例和描述                                                   |
| :------- | :----------------------------------------------------------- |
| `RPUSH`  | `RPUSH key-name value [value ...]`——将一个或多个值推入到列表的右端 |
| `LPUSH`  | `LPUSH key-name value [value ...]`——将一个或多个值推入到列表的左端 |
| `RPOP`   | `RPOP key-name`——移除并返回列表最右端的元素                  |
| `LPOP`   | `LPOP key-name`——移除并返回列表最左端的元素                  |
| `LINDEX` | `LINDEX key-name offset`——返回列表中偏移量为`offset`的元素   |
| `LRANGE` | `LRANGE key-name start end`——返回列表从`start`偏移量到`end`偏移量范围内的所有元素，包括`start`和`end` |
| `LTRIM`  | `LTRIM key-name start end`——对列表进行修剪，只保留从`start`偏移量到`end`偏移量范围内的元素，包括`start`和`end` |

ltrim和lrange结合可以构建一个功能上类似与LPOP或RPOP的操作，并能够一次返回多个弹出的元素。

**阻塞式列表弹出命令以及在列表间移动元素的命令**



| 命令         | 用例和描述                                                   |
| :----------- | :----------------------------------------------------------- |
| `BLPOP`      | `BLPOP key-name [key-name ...] timeout`——从第一个非空列表中弹出位于最左端的元素，或者在`timeout`秒之内阻塞并等待可弹出的元素出现 |
| `BRPOP`      | `BRPOP key-name [key-name ...] timeout`——从第一个非空列表中弹出位于最右端的元素，或者在`timeout`秒之内阻塞并等待可弹出的元素出现 |
| `RPOPLPUSH`  | `RPOPLPUSH source-key dest-key`——从`source-key`列表中弹出位于最右端的元素，然后将这个元素推入到`dest-key`列表的最左端，并向用户返回这个元素 |
| `BRPOPLPUSH` | `BRPOPLPUSH source-key dest-key timeout`——从`source-key`列表中弹出位于最右端的元素，然后将这个元素推入到`dest-key`列表的最左端， 并向用户返回这个元素；如果`source-key`为空，那么在`timeout`秒之内阻塞并等待可弹出的元素出现 |

常用于消息传递(messaging)和任务队列(task queue)的开发





### set



| 命令          | 用例和描述                                                   |
| :------------ | :----------------------------------------------------------- |
| `SADD`        | `SADD key-name item [item ...]`——将一个或多个元素添加到集合里面，并返回被添加元素当中原本并不存在于集合里面的元素数量 |
| `SREM`        | `SREM key-name item [item ...]`——从集合里面移除一个或多个元素，并返回被移除元素的数量 |
| `SISMEMBER`   | `SISMEMBER key-name item`——检查元素`item`是否存在于集合`key-name`里 |
| `SCARD`       | `SCARD key-name`——返回集合包含的元素的数量                   |
| `SMEMBERS`    | `SMEMBERS key-name`——返回集合包含的所有元素                  |
| `SRANDMEMBER` | `SRANDMEMBER key-name [count]`——从集合里面随机地返回一个或多个元素。当`count`为正数时，命令返回的随机元素不会重复； 当`count`为负数时，命令返回的随机元素可能会出现重复 |
| `SPOP`        | `SPOP key-name`——从集合里面移除并返回一个随机元素            |
| `SMOVE`       | `SMOVE source-key dest-key item`——如果集合`source-key`包含元素`item`， 那么从集合`source-key`里面移除元素`item`，并将元素`item`添加到集合`dest-key`中； 如果`item`被成功移除，那么命令返回1，否则返回0 |

smove set1 key1 

用于组合和处理多个集合的Redis命令

| 命令          | 用例和描述                                                   |
| :------------ | :----------------------------------------------------------- |
| `SDIFF`       | `SDIFF key-name [key-name ...]`——返回那些存在于第一个集合、但不存在于其他集合中的元素（数学上的差集运算） |
| `SDIFFSTORE`  | `SDIFFSTORE dest-key key-name [key-name ...]`——将那些存在于第一个集合、但并不存在于其他集合中的元素（数学上的差集运算）存储到`dest-key`中 |
| `SINTER`      | `SINTER key-name [key-name ...]`——返回那些同时存在于所有集合中的元素（数学上的交集运算） |
| `SINTERSTORE` | `SINTERSTORE dest-key key-name [key-name ...]`——将那些同时存在于所有集合的元素（数学上的交集运算）保存到键`dest-key` |
| `SUNION`      | `SUNION key-name [key-name ...]`——返回那些至少存在于一个集合中的元素（数学上的并集计算） |
| `SUNIONSTORE` | `SUNIONSTORE dest-key key-name [key-name ...]`——将那些至少存在于一个集合中的元素（数学上的并集计算）存储到`dest-key`中 |



### hash 

内部编码：`ziplist压缩列表`,`hashtable哈希表`

应用场景：缓存用户信息。

| 命令    | 用例和描述                                                   |
| :------ | :----------------------------------------------------------- |
| `HMGET` | `HMGET key-name key [key ...]`——从散列里面获取一个或多个键的值 |
| `HMSET` | `HMSET key-name key value [key value ...]`——为散列里面的一个或多个键设置值 |
| `HDEL`  | `HDEL key-name key [key ...]`——删除散列里面的一个或多个键值对，返回成功找到并删除的键值对数量 |
| `HLEN`  | `HLEN key-name`——返回散列包含的键值对数量                    |

> 如果哈希元素比较多的话，使用hgetall可能导致redis阻塞。 可以使用hscan，而如果只是获取部分字段，建议使用hmget



| 命令           | 用例和描述                                                   |
| :------------- | :----------------------------------------------------------- |
| `HEXISTS`      | `HEXISTS key-name key`——检查给定键是否存在于散列中           |
| `HKEYS`        | `HKEYS key-name`——获取散列包含的所有键                       |
| `HVALS`        | `HVALS key-name`——获取散列包含的所有值                       |
| `HGETALL`      | `HGETALL key-name`——获取散列包含的所有键值对                 |
| `HINCRBY`      | `HINCRBY key-name key increment`——将键`key`保存的值加上整数`increment` |
| `HINCRBYFLOAT` | `HINCRBYFLOAT key-name key increment`——将键`key`保存的值加上浮点数`increment` |

```python 
>>> conn.hmset('hash-key2', {'short':'hello', 'long':1000*'1'}) # 在考察散列的时候，我们可以只取出散列包含的键，而不必传输大的键值。
True                                                            #
>>> conn.hkeys('hash-key2')                                     #
['long', 'short']                                               #
>>> conn.hexists('hash-key2', 'num')                            # 检查给定的键是否存在于散列中。
False                                                           #
>>> conn.hincrby('hash-key2', 'num')                            # 和字符串一样，
1L                                                              # 对散列中一个尚未存在的键执行自增操作时，
>>> conn.hexists('hash-key2', 'num')                            # Redis会将键的值当作0来处理。
True  
```

### zset

| 命令      | 用例和描述                                                   |
| :-------- | :----------------------------------------------------------- |
| `ZADD`    | `ZADD key-name score member [score member ...]`——将带有给定分值的成员添加到有序集合里面 |
| `ZREM`    | `ZREM key-name member [member ...]`——从有序集合里面移除给定的成员，并返回被移除成员的数量 |
| `ZCARD`   | `ZCARD key-name`——返回有序集合包含的成员数量                 |
| `ZINCRBY` | `ZINCRBY key-name increment member`——将`member`成员的分值加上`increment` |
| `ZCOUNT`  | `ZCOUNT key-name min max`——返回分值介于`min`和`max`之间的成员数量 |
| `ZRANK`   | `ZRANK key-name member`——返回成员`member`在`key-name`中的排名 |
| `ZSCORE`  | `ZSCORE key-name member`——返回成员`member`的分值             |
| `ZRANGE`  | `ZRANGE key-name start stop [WITHSCORES]`——返回有序集合中排名介于`start`和`stop`之间的成员，如果给定了可选的`WITHSCORES`选项，那么命令会将成员的分值也一并返回 |



表3-10 有序集合的范围型数据获取命令和范围型数据删除命令，以及并集命令和交集命令

| 命令               | 用例和描述                                                   |
| :----------------- | :----------------------------------------------------------- |
| ZREVRANK           | `ZREVRANK key-name member`——返回有序集合里成员`member`所处的位置，成员按照分值从大到小排列 |
| ZREVRANGE          | `ZREVRANGE key-name start stop [WITHSCORES]`——返回有序集合给定排名范围内的成员，成员按照分值从大到小排列 |
| ZRANGEBYSCORE      | `ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]`——返回有序集合中，分值介于`min`和`max`之间的所有成员 |
| `ZREVRANGEBYSCORE` | `ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]`——获取有序集合中分值介于`min`和`max`之间的所有成员，并按照分值从大到小的顺序来返回它们 |
| `ZREMRANGEBYRANK`  | `ZREMRANGEBYRANK key-name start stop`——移除有序集合中排名介于`start`和`stop`之间的所有成员 |
| `ZREMRANGEBYSCORE` | `ZREMRANGEBYSCORE key-name min max`——移除有序集合中分值介于`min`和`max`之间的所有成员 |
| `ZINTERSTORE`      | `ZINTERSTORE dest-key key-count key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]`——对给定的有序集合执行类似于集合的交集运算 |
| `ZUNIONSTORE`      | `ZUNIONSTORE dest-key key-count key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]`——对给定的有序集合执行类似于集合的并集运算 |



```python 
>>> conn.zadd('zset-1', 'a', 1, 'b', 2, 'c', 3)                         # 首先创建两个有序集合。
3                                                                       #
>>> conn.zadd('zset-2', 'b', 4, 'c', 1, 'd', 0)                         #
3                                                                       #
>>> conn.zinterstore('zset-i', ['zset-1', 'zset-2'])                    # 因为ZINTERSTORE和ZUNIONSTORE默认使用的聚合函数为sum，
2L                                                                      # 所以多个有序集合里成员的分值将被加起来。
>>> conn.zrange('zset-i', 0, -1, withscores=True)                       #
[('c', 4.0), ('b', 6.0)]                                                #
>>> conn.zunionstore('zset-u', ['zset-1', 'zset-2'], aggregate='min')   # 用户可以在执行并集运算和交集运算的时候传入不同的聚合函数，
4L                                                                      # 共有 sum、min、max 三个聚合函数可选。
>>> conn.zrange('zset-u', 0, -1, withscores=True)                       #
[('d', 0.0), ('a', 1.0), ('c', 1.0), ('b', 2.0)]                        #
>>> conn.sadd('set-1', 'a', 'd')                                        # 用户还可以把集合作为输入传给ZINTERSTORE和ZUNIONSTORE，
2                                                                       # 命令会将集合看作是成员分值全为1的有序集合来处理。
>>> conn.zunionstore('zset-u2', ['zset-1', 'zset-2', 'set-1'])          # 集合和有序集合的并集
4L                                                                      #
>>> conn.zrange('zset-u2', 0, -1, withscores=True)                      #
[('d', 1.0), ('a', 2.0), ('c', 4.0), ('b', 6.0)]  
```



### HyperLogLogs

什么是基数❓

 A = {1, 2, 3, 4, 5}， B = {3, 5, 6, 7, 9}；那么基数，就是**不重复的元素**= 1, 2, 4, 6, 7, 9； （允许容错，即可以接受一定误差） 

HyperLogLogs基数统计用来解决什么问题❓

可以非常节省内存地区统计各种计数，如注册IP数，在线用户数。

### Bitmap

位图数据结构，都是操作二进制位来进行记录，只有0和1两个状态。

用来解决什么问题❓

比如统计用户信息，如登录或没登陆，打卡和没打卡等等.. 



### geospatial



### 底层数据结构

<img src="https://img2018.cnblogs.com/blog/1203840/201812/1203840-20181205100407187-262509023.png" alt="img" style="zoom:50%;" />

- String：如果存储数字，则用int型编码，如果非数字并且小于等于39字节的字符串，则用embstr，大于39个字节，则是raw编码 
- list：如果列表的元素个数小于512个，列表每个元素的指都小于64字节，使用ziplist，否则使用linkedlist编码
- Hash：哈希类型元素个数小于512个，所有值小于64字节，使用ziplist编码；否则使用hashtable编码
- Set：集合中的元素都是整数且个数少于512个，使用intset编码，否则使用hashtable编码
- Zset：有序集合的元素个数小于128个，且每个元素的值小于64字节时，使用ziplist编码，否则使用skiplist(跳跃表)编码



redisObject对象

在redis的命令中，用于对键进行处理的命令占了很大一部分，键能执行的命令又各不相同，如`LPUSH`和`LLEN`只能用于列表键，而SADD和SRANDMEMBER只能用于集合键；而DEL，TTL，TYPE可以用于任何类型的键，但是要正确实现这些命令，必须为不同类型的键设置不同的处理方式。

 以上的描述说明, **Redis 必须让每个键都带有类型信息, 使得程序可以检查键的类型, 并为它选择合适的处理方式**. 

这说明, **操作数据类型的命令除了要对键的类型进行检查之外, 还需要根据数据类型的不同编码进行多态处理**.

为了解决以上问题, **Redis 构建了自己的类型系统**, 这个系统的主要功能包括:

- redisObject对象
- 基于redisObject对象的类型检查
- 基于redisObject对象进行显式多态函数
- 对redisObject进行分配，共享和销毁的机制。

```c
typedef struct redisObject{
 	unsigned type :4 ;
    unsigned encoding:4;
 // LRU - 24位, 记录最末一次访问时间（相对于lru_clock）; 或者 LFU（最少使用的数据：8位频率，16位访问时间）
    unsigned lru:LRU_BITS; // LRU_BITS: 24
    // 引用计数
    int refcount;
    // 指向底层数据结构实例
    void *ptr;
}robj; 
```

- *type记录了对象所保存的值的类型，它的值如下：*

```c
#define OBJ_STRING 0 // 字符串
#define OBJ_LIST 1 // 列表
#define OBJ_SET 2 // 集合
#define OBJ_ZSET 3 // 有序集
#define OBJ_HASH 4 // 哈希表
```

- *encoding记录了对象所保存的值的编码，它的值如下：*

```c
#define OBJ_ENCODING_RAW 0     /* Raw representation */
#define OBJ_ENCODING_INT 1     /* Encoded as integer */
#define OBJ_ENCODING_HT 2      /* Encoded as hash table */
#define OBJ_ENCODING_ZIPMAP 3  /* 注意：版本2.6后不再使用. */
#define OBJ_ENCODING_LINKEDLIST 4 /* 注意：不再使用了，旧版本2.x中String的底层之一. */
#define OBJ_ENCODING_ZIPLIST 5 /* Encoded as ziplist */
#define OBJ_ENCODING_INTSET 6  /* Encoded as intset */
#define OBJ_ENCODING_SKIPLIST 7  /* Encoded as skiplist */
#define OBJ_ENCODING_EMBSTR 8  /* Embedded sds string encoding */
#define OBJ_ENCODING_QUICKLIST 9 /* Encoded as linked list of ziplists */
#define OBJ_ENCODING_STREAM 10 /* Encoded as a radix tree of listpacks */

```

- ptr是一个指针，指向实际保存值的数据结构，这个数据结构由type和encoding属性决定，如一个redisObject的type=OBJ_LIST,encoding=OBJ_ENCODING_QUICKLIST,这个对象就是List
- LRU属性：记录对象最后一次被命令程序访问的时间

空转时间：当前时间 - 键的值对象的lru时间。



命令类型的检查和多态：

> redis是如何处理一条命令的？

当执行一个处理数据类型命令时，redis执行以下步骤：

1. 根据给定的key，在数据库字典中查找和他相对应的redisObject，如果没找到，就返回NULL
2. 检查redisObject的type属性和执行命令所需要的类型是否相符合，如果不符合，返回类型错误
3. 根据redisObject的encoding属性所指定的编码，选择合适的操作函数来处理底层的数据结构
4. 返回数据结构的操作结果作为命令的返回值

 ![img](https://pdai-1257820000.cos.ap-beijing.myqcloud.com/pdai.tech/public/_images/db/redis/db-redis-object-3.png) 



## 发布/订阅模式

场景：下单支付

第一阶段：支付--->库存

第二阶段：支付-->积分--->库存

第三阶段：支付-->积分-->优惠卷-->库存

缺点：

1. 支付业务和其他业务耦合，每当有一个新业务要修改支付结果，就需要改动支付业务
2. 调用业务过多，导致下单接口响应时间变长
3. 如果任一下游接口失败，可能导致数据不一致。

改进思路：支付业务其实不需要关心下游调用结果，只要有某种机制通知到它们就可以了



 Redis提供了基于发布/订阅模式的消息机制，这种机制下的消息发布者和订阅者不需要直接通信。![img](https://img2020.cnblogs.com/other/1419561/202009/1419561-20200923071552234-861298090.jpg)  

| 命令           | 用例和描述                                                   |
| :------------- | :----------------------------------------------------------- |
| `SUBSCRIBE`    | `SUBSCRIBE channel [channel ...]`——订阅给定的一个或多个频道  |
| `UNSUBSCRIBE`  | `UNSUBSCRIBE [channel [channel ...]]`——退订给定的一个或多个频道，如果执行时没有给定任何频道，那么退订所有频道 |
| `PUBLISH`      | `PUBLISH channel message`——向给定频道发送消息                |
| `PSUBSCRIBE`   | `PSUBSCRIBE pattern [pattern ...]`——订阅与给定模式相匹配的所有频道 |
| `PUNSUBSCRIBE` | `PUNSUBSCRIBE [pattern [pattern ...]]`——退订给定的模式，如果执行时没有给定任何模式，那么退订所有模式 |



## 其他命令

### 排序

https://lanjingling.github.io/2015/11/19/redis-sort/

sort可以对链表，集合，有序集合进行排序

内部使用快速排序

| 命令   | 用例和描述                                                   |
| :----- | :----------------------------------------------------------- |
| `SORT` | `SORT source-key [BY pattern] [LIMIT offset count] [GET pattern [GET pattern ...]] [ASC|DESC] [ALPHA] [STORE dest-key]` ——根据给定的选项，对输入列表、集合或者有序集合进行排序，然后返回或者存储排序的结果 |

默认升序，可以指定desc为降序，asc是升序。 

```powershell
127.0.0.1:6379> rpush mylist2 2 34 5 6 11 34 -1 34
(integer) 8
127.0.0.1:6379> lrange mylist2 0 -1
1) "2"
2) "34"
3) "5"
4) "6"
5) "11"
6) "34"
7) "-1"
8) "34"
127.0.0.1:6379> sort mylist2
1) "-1"
2) "2"
3) "5"
4) "6"
5) "11"
6) "34"
7) "34"
8) "34"

```

### 过期

expire key second 

## 持久化

严格意义上的持久化方案：RDB，AOF，虚拟内存VM，DISKSTORE。实际上关键的只有两种RDB和AOF

### RDB

Redis DataBase，快照。在某一时刻将所有数据都写入硬盘中。

触发方式：手动触发和自动触发

rdb是一个紧凑压缩的二进制文件。

**手动触发**

- save命令：*在主线程中操作*。会阻塞当前Redis服务器，在快照创建完毕之前不响应其他任何命令，不常用。
- BGSAVE：(支持BGSAVE的平台)，redis都会调用fork来创建一个子进程来负责将快照写入硬盘，而父进程继续处理命令请求。阻塞只发生在fork阶段，一般时间很短

**自动触发机制**

- redis.conf 中配置 `save m,n`  表示在m秒内有n次修改时，自动触发bgsave生成rdb文件。如save 100,1，表示100s内，有1个key被修改则执行bgsave 
- **主从复制时**，从节点要从主节点进行全量复制时也会触发bgsave操作，生成当时的快照发送到从节点
- **执行debug reload**命令重新加载redis时也会出发bgsave操作
- **默认情况执行shut down命令，关机 **时，如果没有开启AOF，就会出发bgsave操作

在redis.conf中配置rdb

```yaml 
save <seconds>  <changes>
# 默认的设置为： 
save 900 1   表示900s内有1条key信息变化，就进行快照
save 300 10   
save 60 10000

save "" # 表示禁用rdb  n
    
    #RDB 文件的名称 
dbfilename dump.rdb  
    
    #文件保存路径
    dir  /home/work/app/reids/data/
    
    #如果持久化出错，主进程是否停止写入
    stop-writes-on-bgsave-error  yes 
    
    # 是否压缩，默认开启。压缩会消耗cpu 
    rdbcompression  yes

    #导入时是否检查
    rdbchecksum  yes 
```

`stop-writes-on-bgsave-error`:开启时，如果快照操作出现异常，（如操作系统用户权限不够，磁盘空间写满）时，Redis会禁止写操作。 主要目的是运维人员第一时间发现Redis的运行错误。 

`rdbcompression`：该属性将在字符串类型的数据被快照到磁盘文件时，**启用LZF压缩算法**，redis官方建议为yes

`rdbchecksum`：从RDB快照v5版本开始，一个64位的CRC冗余校验编码会被放在RDB文件末尾，以便检验rdb文件的完整性，大概损失10%的性能，但获取了更高的数据可靠性。



#### 深入理解RDB

- 在RDB中将内存中的数据同步到磁盘时可能会持续较长时间，这段时间Redis服务一般会收到数据写操作请求，如何保持数据一致性？

  RDB的核心思路是Copy-On-Write，来保证在进行快照操作的这段时间，需要压缩写入磁盘上的数据在内存上不会发生变化， 在正常的快照操作中，一方面Redis主进程会fork一个新得进程专门进行快照，另**一方面这段时间发生的数据变化会以副本方式存放在另一个新的内存区域，待快照结束后才会同步到原来的内存区域**

 ![img](https://pdai-1257820000.cos.ap-beijing.myqcloud.com/pdai.tech/public/_images/db/redis/redis-x-aof-42.jpg) 

https://blog.csdn.net/Muscleape/article/details/105670481

**优点：**

- RDB是某个时间点的快照，默认用LZF算法压缩，压缩后的文件大小远小于内存大小，使用于备份，全量复制等场景。
- Redis加载RDB文件恢复数据要远快于AOF方式。

**缺点**

- RDB方式实时性不够，无法做到秒级的持久化；
- 每次调用bgsave都需要fork子进程，fork子进程属于重量级操作，频繁执行成本较高；
- RDB文件是二进制的，没有可读性，AOF文件在了解其结构的情况下可以手动修改或者补全；
- 版本兼容RDB文件问题；

### AOF

https://juejin.cn/post/6844903902991630349#heading-1

AOF(append only file)。追加文件。 持久化以独立日志的方式记录命令，**并在Redis重启时在重新执行AOF文件中的命令以达到恢复数据的目的。**AOF的主要作用是解决持久化的实时性。

**AOF持久化的实现**

 ![示意图](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/7/30/16c4345eae4aa6cd~tplv-t2oaga2asx-watermark.awebp) 如图所示，AOF的实现可以分为（命令追加）append， 文件写入（write），文件同步（sync），文件重写（rewrite），重启加载（load）

流程如下：

- 开启了AOF后，服务器在执行完一条写命令后，都会以协议格式将执行的写命令追加到服务器的AOF_buf缓冲区
- AOF缓冲区会根据一定策略向硬盘进行同步操作。
- 随着AOF文件越来越大，需要定期对AOF进行重写，达到压缩的目的
- 当Redis重启时，可以加载AOF文件进行数据恢复。

#### 命令追加



#### 文件写入和同步

何时将aof_buf缓冲区写入到AOF文件中，redis提供三种写回策略：

 ![image-20220419183958113](D:\Typora\自服务\redis.assets\image-20220419183958113.png)

redis每次结束一个事件循环之前，都会调用flushAppendOnlyFile函数，判断是否需要将AOF缓冲区中的内容写入和同步到AOF文件中。

`flushAppendOnlyFile`函数的行为由redis.conf配置中的`appendfsync`选项的值来决定，该选项有三个可选值，分别是`always`,`everysec`,`no`

**在redis.conf中配置AOF**

```yaml 
# appendonly参数开启AOF持久化，默认是关的 
appendonly no

# AOF持久化的文件名，默认是appendonly.aof
appendfilename "appendonly.aof"

# AOF文件的保存位置和RDB文件的位置相同，都是通过dir参数设置的
dir ./

# 同步策略
# appendfsync always
appendfsync everysec
# appendfsync no

# aof重写期间是否同步
no-appendfsync-on-rewrite no

# 重写触发配置
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# 加载aof出错如何处理
aof-load-truncated yes

# 文件重写策略
aof-rewrite-incremental-fsync yes

```

`appendfsync`参数项是AOF功能最重要的设置项之一，主要用于设置"真正执行"操作命令向AOF文件中同步的策略。

appendfsync代表了三种不同调用fsync的策略，调用`fsync`越频繁，读写效率就越差。 

> 什么叫做“真正执行“，在linux中为了保证操作系统IO队列的操作效率，`write`会触发延迟写（delayed write）机制，`write`在写入操作系统缓冲区之后直接返回，而提交的IO操作请求一般是放置在Linux Page Cache中的，然后再由linux的策略自行决定写到磁盘的时机，
>
> 而linux的fsync函数，可以强制地将Page Cache待写的数据写到真正的物理设备上，`fsync`将阻塞直到写入磁盘完成后返回，保证了数据持久化。
>
> 

#### AOF重写

**指令： bgrewriteaof** 

随着Redis运行，AOF的内容会越来越多，体积越来越大。为了解决体积膨胀的问题，Redis提供了AOF重写rewrite功能，通过该功能，redis可以创建一个新的AOF文件来替代旧的AOF文件，

AOF文件重写不需要对旧的AOF文件进行读取，分析或者写入操作，而是通过读取服务器当前数据库状态来实现的。首先从数据库中读取键现在的值，然后用一条命令去记录键值对，代替之前记录这个键值的多条命令，这是AOF重写功能实现原理。

 ![示意图](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/7/30/16c4345eb3719dac~tplv-t2oaga2asx-watermark.awebp) 

AOF重写过程是由后台进程`bgrewriteaof`完成的，主线程fork出后台的bgrewriteaof子进程，同时把主线程的内存拷贝一份给子进程，然后`bgrewriteaof`子进程可以在不影响主进程的情况下，重写日志。

**所以在aof重写中，fork进程过程中会阻塞住主进程的**

**重写日志时，有新数据写入怎么办？**

Redis设置了一个AOF重写缓冲区，这个缓冲区在给服务器创建子进程之后开始使用，当Redis执行完一个写命令之后，他会同时发送给AOF缓冲区和AOF重写缓冲区。 

在子进程完成AOF重写工作时，它会向父进程发送一个信号，父进程在接收到该信号后，会调用一个信号处理函数执行以下工作：

- 将AOF重写缓冲区中的所有内容写入到新的AOF文件中，保证新AOF文件保存的数据库状态和服务器当前状态一致。
- 对新的AOF文件进行改名，原子地覆盖现有AOF文件，完成新旧文件的替换。
- 继续处理客户端请求命令。

 在整个AOF后台重写过程中，只有信号处理函数会对redis主进程进行阻塞，其他时候都不会阻塞主进程。![示意图](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/7/30/16c4345ecdaab9ca~tplv-t2oaga2asx-watermark.awebp) 



### 区别

|                                   | RDB                                        | AOF                                                |
| --------------------------------- | ------------------------------------------ | -------------------------------------------------- |
| 持久化方式                        | 定时对整个内存进行快照                     | 记录每一次执行的命令                               |
| 数据完整性                        | 不完整，两次备份之间会丢失                 | 比较完整，取决于刷盘策略(fsync)                    |
| 文件大小                          | 会有压缩，文件小                           | 记录命令，文件很大                                 |
| 宕机恢复速度                      | 快                                         | 慢（要执行命令）                                   |
| 数据恢复优先级(redis重启先用哪个) | 低，因为数据完整性不如AOF                  | 高，因为数据完整性更高                             |
| 系统资源占用                      | 高，消耗大量CPU和内存                      | 低，主要是磁盘资源，但AOF重写时会消耗大量CPU和内存 |
| 使用场景                          | 可容忍数分钟的数据丢失，追求更高的启动速度 | 对数据安全性要求更高。                             |









## 事件机制

Redis服务器是一个事件驱动程序，服务器需要处理两类事件：

- 文件事件file event：主要指客户端向服务器发送命令，如读写命令和连接命令。
- 时间事件time event ：时间事件指的是定时执行的任务，如serverCron函数。

> redis采用事件驱动机制来处理大量网络IO，他并没有使用libevent或libev这样成熟开源的方案，而使自己实现了一个非常简洁的事件驱动库ae_event

 ![事件管理器示意图](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/8/16c71c3f68a4bf5f~tplv-t2oaga2asx-watermark.awebp) 

`avEventLoop`是整个事件驱动的核心，它管理者文件时间表和时间事件表，不断循环处理着就绪的文件事件和到期的时间事件。

### 文件事件



## 消息队列

Stream 通过XADD和XREAD完成最简单的生产，消费模型

![image-20220627155148841](D:\Typora\自服务\redis.assets\image-20220627155148841.png)

![image-20220627155609899](D:\Typora\自服务\redis.assets\image-20220627155609899.png)

XREAD COUNT 5 BLOCK 0 STREAMS queue 0

代表从queue这个消息队列中获取5条消息，0代表从头开始读取，$代表读取最新的消息。 BLOCK代表阻塞等待，后面数字单位是毫秒，0代表无休止等待。

这样就是一个朴素的消息队列，它存在消息漏读的问题：当我们指定起始id为$时，代表读取最新的消息，如果我们处理一条消息的过程中，又有超过一条以上的消息到达队列，则下一次获取也只能获取最新一条，就会出现漏读消息。



**消费者组**

Consumer Group 将多个消费者划分到一个组中，监听同一个队列。有以下特点：

1. 消息分流

   队列中的消息会分流给组内的不同消费者，而不是重复消费，从而加快消息处理的速度。

2. 消息标识

   消费者组会维护一个标示，记录最后一个被处理的消息，之后即使消费者宕机重启，还会从标识之后读取消息，确保每个消息都会被消费。

3. 消息确认

   消费者获取消息后，消息处于pending状态，会被存入一个pending-list，当处理完成后，需要通过XACK来确认消息，标记消息为已处理，才会从pending-list移除。



创建消费者组：

XGROUP CREATE key groupName ID [MKSTREAM]

Key:消息队列的名称

groupName：消费者组名称

ID：起始ID标识，$表示队列中最后一个消息，0表示开头的消息

MKSTREAM：队列不存在时自动创建队列。

例子：`xgroup create s1 group1 0` 

```
XGROUP DESTROY key groupName #删除指定的消费者组
#给指定的消费者组添加消费者
XGROUP CREATECOMSUMER key groupname  consumerName 
#删除消费者组里指定的消费者
XGROUP DELCONSUMER key groupname consumerName
```

从消费者组读取消息：

XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] [NOACK] STREAMS key [key ...] ID [ID ...] 

group: 消费者组的名称

consumer：消费者的名称，如果消费者不存在，就自动创建一个

count：本次查询的最大数量

BLOCK ： 阻塞等待的最大时长，不给就是不阻塞，

STREAMS key： 消息队列的名称

ID ：获取消息的起始ID ， `>`代表从下一个未消息开始

其他：根据id从pending-list中获取已消费但未确认的消息，例如0



读完消息后，还要给出确认，用XACK指令：

XACK key group ID [ID ...]



查看Pending-list里的消息

XPENDING key group [IDLE min-idle-time]

## 高可用

高可用High Availabity,是分布式系统架构设计中必须考虑的因素之一，**它通常是指通过设计减少系统不能提供服务的时间**

**大型系统的分层架构及物理服务器的分布式部署使得位于不同层次的服务器具有不同的可用性特点。关闭服务或服务器宕机时产生的影响也不相同，高可用的解决方案也差异甚大**。大致可以分为：

- 高可用的应用 - 主要手段是：负载均衡
- 高可用的服务 - 主要手段是：分级管理、超时重试、异步调用、限流、降解、断路、幂等性设计
- 高可用的数据 - 主要手段是：数据备份和失效转移

**CAP原理**

C：Consisten 一致性

A：Availability 可用性

P：Partition tolerance 因网络故障或其他原因导致分布式系统的某些节点与其他节点失去连接，形成独立分区。 

> 分布式系统的结点往往都是分布在不同机器上进行网络隔离开的，这意味着必然会有网络断开的风险，这个网络断开的场景的专业词汇叫做网络分区。
>
> 网络分区发生时，两个结点无法通信，对一个节点的修改无法同步到另一个结点，所以数据的一致性无法满足，除非牺牲可用性，即暂停结点的服务，在网络分区发生时，不再提供修改功能直到网络恢复。
>
> 总结：网络分区发生时，无法同时保证一致性和可用性。

#### BASE理论

基本可用Basically Available，分布式系统故障时，允许部分损失可用性，保证核心可用。

软状态 Soft State：允许中间状态，即允许数据有一段时间不一致，但最终会保证一致性。

最终一致性 Eventual Consistency：最终一致性，即软状态结束后，保证最终一致性 

BASE理论是对CAP的一种解决思路。 



### 主从复制

https://zhuanlan.zhihu.com/p/151740247

redis的主从数据是异步同步的，所以分布式redis**只保证最终一致性**，满足的是可用性。从节点会用多种策略努力追赶落后的数据，继续保持和主节点一致。

指将一台redis服务器的数据复制到其他redis服务器，前者叫master主节点，后者称slave结点，数据复制是单向的，只能由主->从。

作用：

- 数据冗余：实现了数据热备份，是持久化之外的一种数据冗余方式
- 故障恢复：由主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；
- 负载均衡：在主从复制基础上，**配合读写分离**，可以由主节点提供写服务，由从节点提供读服务，分担服务器负载，尤其是写少读多的场景下， 通过多个从节点分担负载，可以大大提高redis服务器的并发量。
- 高可用基石：主从复制是哨兵和集群能够实施的基础。

Redis主从同步**最终一致性：**

redis的主从数据是异步同步的，所以分布式redis系统不满足一致性要求，而满足可用性要求。

从结点会努力追赶主节点，最终主从结点的状态保持一致。如果网络断开了，主从结点的数据会出现大量不一致，一旦网络恢复，从节点会采用多种策略努力追赶落后的数据，继续尽力保持和主结点的一致。



> 在2.8以前，从节点发送sync到主节点请求同步数据的方式是全量复制。 
>
> 2.8后从节点可以发送psync命令请求同步数据，此时根据主从结点当前状态的不同，同步方式可能是全量复制或者部分复制。 

#### 全量复制

**用于初次复制或者其他无法进行部分复制的情况，将主节点中的所有数据都发生给从节点，是一个重型的操作。**



Redis通过`psync`命令进行全量复制过程如下：

1、从节点判断无法进行增量复制，于是发送全量复制的请求；或者从节点发送增量复制请求，但主节点判断无法进行增量复制，于是进行全量复制。

2、主节点收到全量复制命令后，执行`bgsave`,在后台生成RDB文件，并使用一个缓冲区（叫做复制缓冲区）记录现在开始执行所有写命令

3、主节点`bgsave`完成后,将RDB文件发送给从节点，从节点首先清除自己的旧数据，加载RDB文件，将数据库状态更新到主节点执行bgsave时的数据库状态。

（4）主节点将前述复制缓冲区中的所有写命令发送给从节点，从节点执行这些写命令，将数据库状态更新至主节点的最新状态

（5）如果从节点开启了AOF，则会触发bgrewriteaof的执行，从而保证AOF文件更新至主节点的最新状态

![image-20220420220108265](D:\Typora\自服务\redis.assets\image-20220420220108265.png)

通过全量复制的过程可以看出，**全量复制是非常重型的操作**：

（1）主节点通过`bgsave`命令`fork`子进程进行RDB持久化，该过程是非常消耗CPU、内存(页表复制)、硬盘IO的；

（2）主节点通过网络将RDB文件发送给从节点，对主从节点的带宽都会带来很大的消耗

（3）从节点清空老数据、载入新RDB文件的过程是阻塞的，无法响应客户端的命令；如果从节点执行bgrewriteaof，也会带来额外的消耗

#### 增量复制

redis2.8开始提供增量复制，用来处理网络中断时的数据同步。

增量复制的实现依赖三个重要概念：

1. 复制偏移量

   主从节点分别维护一个复制偏移量offset，主节点向同步了N字节数据后，将修改offset=offset+N，同理，从节点同步后也修改成offst+N。

   `offset`用来判断主从数据库是否一致，如果相同则一致，如果不同，可以根据offset找出缺少的从节点那一部分数据。例如，主节点offset=1000，从节点offset=500，那么要复制的就是offset=501~1000的部分

2. 复制积压缓冲区

   主从节点内部维护一个**固定长度的**、**FIFO队列**作为复制积压缓冲区，默认为1MB，在主节点进行命令传播时，不仅会将写命令同步到从节点，还将写命令写入复制积压缓冲区。

   由于复制积压区是先进先出，他保存的是主节点最近执行的写指令，时间较早的写指令会被积压出去，因此，当主从节点offset的差距过大超过缓冲区长度时，将无法执行部分复制，只能执行全量复制。



> 从节点将offset发送给主节点后，主节点根据offset和缓冲区大小决定是否能进行部分复制：
>
> - 如果offset偏移量后的数据依然在复制积压缓冲区里——部分复制
> - 如果offset偏移量后的数据不再复制积压缓冲区里——全量复制。

 ![img](https://pic4.zhimg.com/80/v2-3aa9eb4881fb69142204c764ceae7b5f_720w.jpg) 





3. 服务器运行ID（runID） 

每个redis结点都有自己运行的ID，运行ID由结点在启动时自动生成，主节点会把自己运行的ID发送给从节点，从节点保存起来。

当从节点断开重连时，就根据运行ID判断同步进度：

- 如果从节点保存的Runid=主节点现在的Runid，说明主从结点之前同步过，主节点会继续尝试部分复制 *(是尝试，因为还得看offset和复制积压缓冲区的情况)*
- 如果从节点保存的runid 不等于 主节点现在的runid，说明在从节点断线同步前的Redis结点不是当前的主节点，只能进行全量复制。



#### psync命令的执行

 ![img](https://pic1.zhimg.com/80/v2-4b654ec5da237888f60905859ef4ae94_720w.jpg) 



从节点：

- 如果从节点没有复制过任何主节点或者执行过`slaveof no one`，从节点会发送`psync`命令，请求全量复制
- 如果前面执行过同步，从节点会发送`psync{runid} {offset}`命令给主结点，其中runid是上一次主节点运行的ID，offset是当前从节点的复制偏移量。

主节点收到`psync`命令后，有三种可能：

①、主节点返回fullresync{runid} {offset}，表示主节点要求与从节点进行全量复制

![image-20220331113910289](D:\Typora\自服务\redis.assets\image-20220331113910289.png)



②、主节点返回+continue，表示主节点和从节点会进行部分数据的增量复制操作。

![image-20220331114625156](D:\Typora\自服务\redis.assets\image-20220331114625156.png)

两个数组：`replication buffer` 和 `repl_backlog_buffer`

`replication buffer` ：redis

`repl_backlog_buffer`

③、如果主节点返回-err，表示主节点的redis版本低于2.8，无法识别psync命令，从节点发送`sync`进行全量复制。

#### 心跳检测机制



### 哨兵机制

Redis Sentinel ，即Redis哨兵。Redis2.8引入，核心功能是主节点的故障自动转移。所有功能如下：

- 监控： 哨兵会不断检查主结点和从节点是否运作正常
- 自动故障转移：主节点不能正常工作时，哨兵会自动故障转移操作，它会将失效主节点的其中一个从节点升级为新的主节点。 
- 配置提供者：客户端初始化时，通过连接哨兵来获得当前redis服务主节点地址。（类似于发现服务）
- 通知：哨兵可以将故障转移结果发送给客户端。

 ![image-20220420230635913](D:\Typora\自服务\redis.assets\image-20220420230635913.png) 







#### 主结点下线的判定

哨兵通过心跳机制监控redis主从结点

- 主观下线： 任何一个哨兵通过💓发现一个主节点没有回应，可以做出redis结点下线的判断
- 客观下线：当某个哨兵发现了主库“主观下线”后，就会给其他哨兵发送`is-master-down-by-addr`命令，其他哨兵会根据自己和主库的连接情况，投票发出Y或N、当赞同投票数大于等于哨兵配置文件中`quorum`这个配置项（一般设置为哨兵数量的一半）时，就可以判定为主库客观下线了



#### 选举主结点

- 先过滤点不健康、没回复过哨兵ping响应的从节点
- 选择`slave-priority`从节点优先级最高的
- 选择offset(复制偏移量最大的)，即复制最完整的从节点。 

#### 哨兵集群的选举

> 为了避免哨兵的单点情况发生，所以需要一个哨兵的分布式集群。作为分布式集群，必然涉及共识问题（即选举问题）；同时故障的转移和通知都只需要一个主的哨兵节点就可以了。

  

Raft选举算法：*选举的票数大于等于num(sentienls)/2+1*时，将成为领导者，否则继续选举。 

任何一个想成为leader的哨兵，要符合两个条件：

- 要拿到半数以上的赞成票
- 拿到的票数还要大于等于哨兵配置文件中的quorum值

假设有3个哨兵，quorum=2，那么任何一个想成为leader的哨兵只需要拿到2张赞成票。

进一步理解：*判断客观下线 VS哨兵选举*

Redis1主4从，5个哨兵，quorum=2，挂了3个哨兵，当主库宕机时，哨兵能否判断主库“客观下线”，能否自动切换？

1. 可以判断客观下线，有2个哨兵认为主观下线，达到了quorum值，即可以判断“客观下线”
2. 但不能完成主从切换， 在选举哨兵leader时，一个哨兵必须获得（5/2+1)=3张票，但此时只有2个哨兵活着。

#### 哨兵集群的组建

哨兵实例之间可以互相发现，要归功于Redis提供的pub/sub机制。

在主从集群上，主库有一个名为`__sentinel__:hello`的频道，不同哨兵就是通过它进行相互发现的。 



![image-20220421101819703](D:\Typora\自服务\redis.assets\image-20220421101819703.png)

如图： 哨兵1把自己的ip和端口发布到频道上， 哨兵2，3就可以进行订阅消息并拿到哨兵1的信息，就可以通过网络通信和哨兵1进行连接。 同理，哨兵2，3也可以用同样的方法互相发现。



### 分片集群

> 通过持久化，主从复制和哨兵集群，Redis保障了高并发读的性能。 但对于单个redis节点的存储能力（单节点的存储如果不能太大，因为rdb文件也会很大，主从同步消耗时就很大），和redis集群高并发写的能力依然有不足。 
>
> 海量数据存储问题，高并发写问题。 



### 多级缓存

> 从用户请求数据到数据返回，数据经过了浏览器，CDN，代理服务器(nginx)，应用服务器(tomcat),以及数据库各个环节，每个环节都可以运用缓存技术。 



 缓存的请求顺序是：用户请求 → HTTP 缓存 → CDN 缓存 → **代理服务器(NGINX)缓存 → 进程内缓存 → 分布式缓存 → 数据库。**

整个多级缓存系统分为三层： 

- 应用层Nginx缓存
- 分布式Redis缓存集群
- Tomcat堆内缓存 , 又叫JVM进程缓存

  

 ![img](http://moguhu.com/images/8929d5dc-d3dc-44a8-89a8-833b0e5512ed.png) 

如图：

1. 接入层Nginx将请求负载均衡到应用层Nginx

2. 应用层Nginx首先访问Local Cache(Lua Shared Dict,Nginx Proxy Cache等)，如果命中则直接返回，Nginx本地缓存对热点数据非常有效

3. 如果没命中，则会读取分布式缓存如Redis

4. 如果分布式缓存没命中，则访问Tomcat集群。

5. Tomcat读取本地缓存，有则返回并写入到Redis集群，如没有就去DB查询。 

   

```
docker run -p 3306:3306   \
--name mysql \
-v $PWD/conf:/etc/mysql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:/var/lib/mysql  \
-e MYSQL_ROOT_PASSWORD=123456\
-d \
--privileged  mysql:5.7.25 

```

【Linux安装redis】

https://www.cnblogs.com/hunanzp/p/12304622.html

https://segmentfault.com/a/1190000023364209

```
  mysql -h127.0.0.1  -uroot -p123456
```

## 缓存问题

### 缓存穿透

缓存穿透指客户端请求的数据在缓存和数据库中都不存在，这样缓存永远不会生效，这些请求都打到数据库中。

解决方法：

**缓存空对象：**在缓存中存储空对象。 

优点：实现简单，维护方便

缺点：额外的内存消耗-->设置过期时间

可能造成短期不一致。 

**布隆过滤器：**在客户端和Redis之间加一个布隆过滤器。

实际上是算法，客户端请求先到布隆过滤，如果不在就直接返回。

- 内存占用少，没有多余的key
- 实现复杂，存在误判可能。 



备注： 还可以使用主动的防御措施，比如

ID设计复杂高的，有一定规律性可以拦截的。

增加用户权限校验



  



### 缓存雪崩

指同一时间段**大量的缓存Key同时失效或者Redis服务器宕机**，导致大量请求打到数据库 。 

解决方案：

-  缓存数据的过期时间设置随机，防止同一时间大量数据过期。
- 如果是分布式存储，则将热点数据均匀分到不同的缓存数据库
- 设置热点数据永远不过期。 









### 缓存击穿

也叫做热点key问题，

一个**被高并发访问**并且缓存重建业务比较复杂的，在缓存重建过程中，无数请求访问会瞬间给数据库巨大的冲击。 

(数据可能需要多表查询才能全部查出来，并且由多个线程都因为查不到缓存而去数据库做查询） 

解决方案：

1. 互斥锁
2. 逻辑过期

**🔒互斥锁**

获得互斥锁的线程才能去访问数据库。其他的阻塞等待。 

**🥑逻辑过期**

不再使用Redis的TTL时间，而是在存储数据时增加一个expire字段表示过期。当有线程来查询数据并发现过期时，将新开一个子线程，让子线程去获取互斥🔒查询数据库并更新，原线程直接返回已经过期的数据。此时如果有其他线程来查询数据，发现过期并尝试获取锁，但获取不成功，也直接返回过期数据。 



|      | 互斥锁                     | 逻辑过期                                               |
| ---- | -------------------------- | ------------------------------------------------------ |
| 优点 | 保证一致性                 | 性能好                                                 |
| 缺点 | 性能较差、可能出现死锁问题 | 实现复杂，使用过期数据，数据一致性差。有额外的内存消耗 |



### 缓存淘汰策略



[缓存更新的套路](https://coolshell.cn/articles/17416.html)



Redis的淘汰策略有8种，可以分成三类：

1、不淘汰

- noeviction

2、对设置了过期时间的数据进行淘汰

- 随机淘汰：voliatile-random
- ttl:voliatile-ttl
- lru:volatile-lru
- lfu:volatile-lfu

3、全部数据进行淘汰：

- 随机：allkeys-random
- lru：allkeys-lru
- lfu：allkeys-lfu



①、noeivtion

Redis的默认策略。这种策略下，如果缓存满了，再有请求过来时，Redis不再提供服务而是直接返回错误。无法解决缓存污染问题，一般生产环境不建议使用

②、volatile-random

在设置了过期时间的键值对中，进行随机删除。这种方法无法把不在访问的数据筛选出来，所以可能依然存在缓冲污染问题。

③、volatile-ttl

Redis在筛选需要删除的数据时，越早过期的数据越优先删除。

④、volatile-lru

使用了LRU（Least Recentyl Used）算法：按照最近最少使用的原则来筛选过期了的键值对。

**Redis对LRU算法的实现：**



















## Redis事务

`MULTI`,`EXEC`,`DISCARD`,`WATCH`是Redis事务的基础

🥑MULIT

用于开启一个事务。

> MULTI 执行之后， 客户端可以继续向服务器发送任意多条命令， 这些命令不会立即被执行， 而是被放到一个队列中， 当 EXEC 命令被调用时， 所有队列中的命令才会被执行。



🥑EXEC

EXEC命令负责触发并执行事务中的所有命令：

- 如果客户端在使用MULTI开启一个事务后，却因为断线没有成功执行EXEC，那么事务中的所有命令都不会被执行
- 另一方面，如果客户器成功开启事务后执行EXEC，那么事务中的所有命令都会执行。

🥑DISCARD

执行此命令时，事务会被放弃，事务队列会被清空，并且客户端会从事务状态中退出。

## pipeline

redis客户端执行一条命令分为4个过程：

发送命令—命令排队—命令执行—返回结果

这个过程称为Round trip time 简称RTT（往返时间）

 <img src="https://img-blog.csdnimg.cn/20181122105251930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxbGd5,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:50%;" /> <img src="https://img-blog.csdnimg.cn/20181122105343203.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxbGd5,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:50%;" />  

管道可以一次性发送多条命令并在执行完后一次性将结果返回，pipeline通过减少客户端和Redis的通信次数来降低RTT。 



开启Redis Pipeline后，命令不会立即发送给服务端，而是先暂存在客户端，等到所有命令都执行完，才会统一发送给服务端。 

服务端会根据发送过来的命令的顺序，依次运行计算。

然后同样先将结果暂存服务端，等到命令都执行完毕之后，统一返回给客户端。



 ![img](https://s6.51cto.com/oss/202103/10/9b7921bfe51af9d63552aa8f43803b5f.png) 



**原生批命令（mset，mget）与Pipeline对比：**

原生批命令是原子性的，pipelie是非原子性的。

原生批命令是服务端实现的，pipelie需要服务端和客户端共同完成的。并且实际执行过程中，Redis pipeline 会根据需要发送命令数据量大小进行拆分，拆分成多个数据包进行发送。



### lua脚本

Redis使用lua脚本的好处：

- 原子操作：lua脚本是作为一个整体执行的，中间不会被其他命令插入

- 减少网络开销：可以将多个请求通过脚本一次性发送，减少网络时延
- 复用性：lua脚本可以常驻在redis内存中，在使用时可以直接拿来复用，也减少了代码量。

🥑🥑**Eval命令**

` EVAL script numkeys key [key ...] arg [arg ...] `

- script: 脚本程序
- numkeys: 指定键名参数的个数，当脚本不需要任何参数时，也不能省略这个参数（设为0）
- key [key ...]：从EVAL的第三个参数开始算起，表示在脚本中所用到的那些Key，这些键名参数可以在Lua钟通过全局变量KEYS数组，以1为起始下标访问
- arg [arg …]： 附加参数，在 Lua 中通过全局变量 ARGV 数组访问，访问的形式和 KEYS 变量类似( ARGV[1] 、 ARGV[2] ，诸如此类)。





EVAL "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}"  2 username age jack 20 



在 Lua 脚本中，可以使用两个不同函数来执行 Redis 命令，它们分别是redis.call()和redis.pcall()， 两个函数的参数可以是任何格式良好(well formed)的 Redis 命令。

比如：

`eval "return redis.call('set','foo','bar')" 0`
虽然上面这段脚本的确实现了将键 foo 的值设为 bar 的目的,但是它**违反了 EVAL 命令的语义**，因为脚本里使用的所有键都应该由 KEYS 数组来传递，就像这样：
`eval "return redis.call('set',KEYS[1],'bar')" 1 foo`



区别：
  call()和pcall()很类似，唯一的区别是对错误处理的不同。

  当 redis.call() 在执行命令的过程中发生错误时，脚本会停止执行，并返回一个脚本错误，错误的输出信息会说明错误造成的原因；

  redis.pcall() 出错时并不引发(raise)错误，而是返回一个带 err 域的 Lua 表(table)，用于表示错误，但是仍继续执行。

[部分原理剖析](https://www.51cto.com/article/649167.html)

## springBoot-redis

spring操作redis 有三种方案： spring data redis，jedis， 

### spring data redis

Spring Data Redis针对Redis提供了模板RedisTemplate,提供了如下功能：

1. 连接池自动管理，提供了一个高度封装的RedisTemplate类
2. 针对jedis客户端的中的大量api进行归类封装，将同一类型操作封装为operation接口。

- ValueOperations: 简单的K-V操作
- SetOperations: Set类型数据操作
- ZSetOperations: ZSet类型数据操作
- HashOperations: 针对map类型的数据操作
- ListOperations: 针对list类型的数据操作

```xml 
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
<!--线程池-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```



StringRedisTemplate VS RedisTemplate

- StringRedisTemplate 继承了RedisTemplate

- RedisTemplate默认采用的是String的序列化策略，保存的key和value都是采用此策略序列化的
- StringRedisTemplate 默认采用JDK的序列化策略







