Redis

Redis 高性能 key-value 数据库。

#### Redis 数据类型

**String（字符串）**：maxlength=512MB

```shell
SET key value  # 添加键值对
GET key        # 取值
```

**Hash（哈希）**：键值(key=>value)对集合，是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。

```shell
HSET hashName key1 value1 key2 value2... # 设置hash
HGET hashName key # 取值
```

**List（列表）**：按插入顺序。

```shell
lpush listName value1
lpush listName value2
lpush listName beginIndex endIndex
...
```

**Set（集合）**：string类型的无序集合。集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

```shell
sadd key member # 添加元素
smembers key    # 查看所有集合元素
```

**zset（sorted set：有序集合）**：zset 和 set类似，不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

```shell
zadd key score member # 添加元素
```

各个数据类型应用场景：

| 类型                 | 简介                                                   | 特性                                                         | 场景                                                         |
| -------------------- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| String(字符串)       | 二进制安全                                             | 可以包含任何数据,比如jpg图片或者序列化的对象,一个键最大能存储512M | ---                                                          |
| Hash(字典)           | 键值对集合,即编程语言中的Map类型                       | 适合存储对象,并且可以像数据库中update一个属性一样只修改某一项属性值(Memcached中需要取出整个字符串反序列化成对象修改完再序列化存回去) | 存储、读取、修改用户属性                                     |
| List(列表)           | 链表(双向链表)                                         | 增删快,提供了操作某一段元素的API                             | 1,最新消息排行等功能(比如朋友圈的时间线) 2,消息队列          |
| Set(集合)            | 哈希表实现,元素不重复                                  | 1、添加、删除,查找的复杂度都是O(1) 2、为集合提供了求交集、并集、差集等操作 | 1、共同好友 2、利用唯一性,统计访问网站的所有独立ip 3、好友推荐时,根据tag求交集,大于某个阈值就可以推荐 |
| Sorted Set(有序集合) | 将Set中的元素增加一个权重参数score,元素按score有序排列 | 数据插入集合时,已经进行天然排序                              | 1、排行榜 2、带权重的消息队列                                |



#### Redis Key

```shell
PING  # ping
```

Redis Keys 命令

```shell
KEYS patten # 查找符合模式的key
EXISTS key # 检测key是否存在
MOVE key db #移动key到给定数据库中
DEL key  # 删除key
DUMP key # 序列化key
RANDOMKEY # 随机返回一个key
RENAME key newKey # 重命名key
RENAMENX key newKey # 仅当key不存在时，重命名key
TYPE key # 返回key的值的类型

EXPIRE key seconds     # 设置过期时间（以秒计）
EXPIREAT key timestamp # 设置过期时间（UNIX时间戳）
TTL key # 以秒为单位返回key剩余过期时间
PEXPIRE key milliseconds # 设置过期时间（以毫秒计）
PEXPIREAT key milliseconds-timestamp # 设置过期时间（UNIX时间戳以毫秒计）
PTTL key # 以毫秒为单位返回key剩余过期时间
PERSIST key # 持久化key，移除其过期时间
```

Redis String

Redis Hash

Redis List

Redis Set

Redis ZSet

#### Redis 发布订阅

Redis 发布订阅（pub/sub）