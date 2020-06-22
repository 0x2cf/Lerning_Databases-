# Redis:

## What is？

> Redis：REmote DIctionary Server (远程字典服务器)
>
> 是一个高性能的（key/value）分布式内存数据库，基于内存并支持持久化的NoSQL数据库，结构服务器
>
> Redis特点：
>
> - Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可用再次加载进行使用
> - Redis不仅仅支持简单的key-value类型的数据，同时还支持list，set，zset，hash等数据结构的存储
> - Redis**支持数据的备份**。即master—slave 模式的数据备份

## How  to do？

> - 内存存储和持久化，redis支持异步将内存中的数据写到硬盘上，同时不影响继续服务
> - 取最新N个数据的操作，eg：将最新评论的ID放在Redis的List集合里面
> - 模拟类似于HttpSession这种需要设定过期时间的功能
> - 发布、订阅消息系统
> - 定时器、计数器

## Where download？

> Http://redis.io/    	//官方文档
>
> http://www.redis.cn/ 	// 中文文档

## How to Use？

> 数据类型、基本操作和配置
>
> 持久化和复制，RDB/AOF
>
> 事物的控制(部分)
>
> 复制