# Aggregate:

******

## 单值多value：

******

### sadd/smembers/sismember：

![Redis-Aggregate](D:\Boke\Databases\Nosql\Redis\Basic\Redis-Aggregate.png)

### scard 获取集合里面的个数

srem key value 删除集合中元素

![Redis-Aggregate1](D:\Boke\Databases\Nosql\Redis\Basic\Redis-Aggregate1.png)



srandmember key number 随机取出number个数

![Redis-Aggregate2](D:\Boke\Databases\Nosql\Redis\Basic\Redis-Aggregate2.png)

### spop 随机出栈

### smove key1 key2 在key1的某个值 作用将key1 李的某个值赋给 key2

![Redis-Aggregate3](D:\Boke\Databases\Nosql\Redis\Basic\Redis-Aggregate3.png)

## 数学集合：

![Redis-Aggregate4](D:\Boke\Databases\Nosql\Redis\Basic\Redis-Aggregate4.png)

### 差集sdiff：在第一个a1 里面不在a2的值

### 交集sinter: 在a1也在a2里面值

### 并集 sunion：在a1和a2 里面所有的值