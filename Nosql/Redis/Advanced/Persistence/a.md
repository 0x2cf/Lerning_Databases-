# Persistenced：

https://www.cnblogs.com/zxs117/p/11242026.html

> 检测端口是否运行
>
> lsof -i ：6379
>
> //  查看内存使用情况
>
> free -m
>
> // 查看磁盘空间
>
>  df -h

## RDB（Redis Database）：

### 简介：

将时间段间隔内的内存数据以快照的形式写入磁盘，它恢复时是将快照文件直接读到内存里（snapshot）

### 原理：

Redis 会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，在用这个临时文件替换上次持久化号的文件。**主进程是不进行任何的IO操作，确保了极高的性能**。如果需要进行大规模的数据恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能会**丢失**

### Fork：

> 作用：创建一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等）都与原进程一致。但为一个全新的进程，并作为原进程的子进程

RDB保存的是dump.rdb文件

### 配置位置：

在redis.conf 中的snapshoting

### 快照：

> save，save只管保存，其他不管全部阻塞
>
> bgsave：redis会在后台异步进行快照操作
>
> 快照同时还可以响应客户端请求，可以通过lastsave命令获取最后一次成功执行快照的时间



> 执行flush命令也会产生dump.rdb文件，但里面是空的，无意义

### 如何恢复：

> 将备份文件（dump.rdb）移动到redis安装目录，并启动服务即可
>
> 在连接完成之后的终端 使用  **config get dir** 获取目录
>
> 异常恢复:
>
> redis-check-rdb --fix {}

优势：

> 适合大规模的数据备份
>
> 对数据完整性和一致性要求不高

劣势：

> 在一定时间间隔内做一次备份，所以如果redis意外down了就会丢失最后一次快照后的所有更改
>
> Fork的时候，内存中的数据被克隆了一份，内存等将会2倍膨胀性，需考虑！！！

### 如何停止：

​	动态所有停止rdb保存规则方法：redis-cli config set save “”

![Redis_RDB](D:\Boke\Databases\Nosql\Redis\Advanced\Persistence\Redis_RDB.png)

## AOF：（Append only File）：

## 导入：

> 为什么还会出现AOF？（新技术的出现必定弥补老技术的不足,新技术一定会借鉴老技术,是老技术的子类）
>
> 如果一个系统里面同时存在RDB是冲突呢还是协作?
>
> 为什么AOF会在RDB之后产生
>
> AOF它会有什么优缺点?

原理:**以日志的形式来记录每个写操作**,将Redis执行过的所有写**指令记录下来**(读操作不记录),只需追加文件但不可改写文件,Redis启动之初会读取该文件重新构建数据,  换言之,redis重启就根据日志文件的内容将写指令从前到后执行一次以完成数据恢复工作

配置位置:

redis.conf中的APPEND ONLY MODE

配置说明:

Appendfsunc:

> always: 同步持久化,每次发生更改立即记录到磁盘,性能差但数据完整性比较好
>
> everysec:出厂默认推荐,异步操作,每秒记录,如果一秒内宕机,有数据丢失
>
> No

No-appendfsy-on-rewrite:重写时是否可以运用Appendfsync,默认no即可,保障数据安全

Auto-aof-rewrite-min-size :设定重写基准值

Auto-aof-rewrite-percentage : 设定重写基准值

![Redis_AOF1](D:\Boke\Databases\Nosql\Redis\Advanced\Persistence\Redis_AOF1.png)



### 探讨dump.rdb,与aof的二者是否能共存及选择顺序

> 当aof损坏时,rdb完全,二者可以和平共存
>
> 二者先选择aof

### AOF启动／修复／恢复:

#### 正常恢复:

启动:将redis.conf中APPEND ONLY MODE 下appendonly no改为yes

将有数据的aof文件复制一份保存到对于目录(config get dir)

恢复: 重启redis然后重新加载即可

#### 异常恢复:

备份被写坏的AOF文件,

修复: Redistribution-check-aof --fix 进行修复

重启redis,重新加载即可

### Rewrite:重写

> AOF采用文件追加的方式，文件会越来越大为避免出现此种情况，新增了重写机制，
>
> 当AOF文件的大小超过所设定的阈值时，Ｒｅｄｉｓ会启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集，可以使用命令ｂｇｒｅｗｒｉｔｅａｏｆ

#### 重新原理：

> AOF文件持续增长而过大时,会fork出一条新进程来讲文件重写(也是先写零食文件最后在rename),遍历新进程的内存数据,每条记录有一条的set语句.重写aof文件的操作,并没有读取旧的aof文件,而是将整个内存中的数据库内容用命令的方式重写了一个新的aof文件.和快照有点类似

触发机制:

REDIS会记录上次重写时的AOF大小,默认配置时当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发

![Redis_AOF2](D:\Boke\Databases\Nosql\Redis\Advanced\Persistence\Redis_AOF2.png)

优势:

> 每秒同步:appendfsync always 同步持久化 每次发送数据变更会被立即记录到磁盘,性能较差完整性比较友好
>
> 每修改同步: appendfsync everysec 异步操作,每秒记录,如果一秒内宕机,有数据丢失
>
> 不同步:appendfsync no 从不同步

劣势:

相同数据集的数据而言aof文件要远大于rdb文件,恢复速度慢于rdb

AOF运行效率要慢于rdb,每秒同步策略效率较好,不同步效率和rdb相同



![Redis_AOF3](D:\Boke\Databases\Nosql\Redis\Advanced\Persistence\Redis_AOF3.png)

## Which one?：

RDB持久化方式:能够在指定的时间间隔内对数据进行快照存储

AOF持久化方式:记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据,AOF命令以Redis协议追加保存每次写的操作到文件末尾,Redis还能对AOF文件进行后台重新,使得AOF文件的体积不至于过大

只做缓存:

> 如果希望数据在服务器运行的时候存在,也可以不使用热河持久化方式.

同时开启两种持久化方式:

> 当redis重启的时候会优先载入AOF文件来恢复原始的数据,因为在通常情况下AOF文件保存的数据集要比rdb文件保存的数据集更完整

> RDB的数据不实时,同时使用两者时服务器重启也指挥找AOF文件
>
> 那干脆直接使用AOF？不建议
>
> 因为RDB更适合用于数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的BUG，留着作为一个万一的手段

![image-20200603173010540](D:\Boke\Databases\Nosql\Redis\Advanced\Persistence\Redis_AOF4.png)

