单进程

默认16各数据库，使用select index 切换数据库

![b.Basicnum](D:\Boke\Databases\Nosql\Redis\Basic\b.Basicnum.png)

> dbsize ：查看所有key的数量
>
> keys *  显示所有的 key
>
> flush {DB/all} 删除key
>
> exists key 检测key是否存在
>
> move keyname index  将key移动到另一数据库
>
> expire key second： 为key设置有效期
>
> ttl key： 查看key剩余过期时间 -1 永不过期， -2 已过期
>
> type key： 查看key是什么类型

set key第一次为添加，第二次为覆盖

incr/decr/incrby/decrby 一定是数字才能进行加减

getrange /set range 范围内取值，范围内设值

setex(set with expire) 键秒值/setnx(set if not exist)

mset/mget/msetnx 多项处理m/more