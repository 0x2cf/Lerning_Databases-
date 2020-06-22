# Zset/sorted set: sorted set

> 多说一句，在set基础上加一个score值
>
> 之前set是k1 v1 v2 v3
>
> 现在是zset 是 k1 score1 v1 socre2 v2

### zadd/zrange ——with socre 

![Redis-Zset1](D:\Boke\Databases\Nosql\Redis\Basic\Redis-Zset1.png)

### zrangescore key 开始score 结束 score：在score1 与 score2 之间的key

> withscores
>
> （ 不包含
>
> Limit 返回限制： 开始下标步 多少步

zrem key 某score下对于的value值 (删除元素)

zcard/zcount key score 区间/zrange key value值 （ 获得下标值/zscore key 对应的值，获取对应值，获得分数）

zrevrange key values值 :逆序获得下标值

zrevrange

zrevrangebysocre key 