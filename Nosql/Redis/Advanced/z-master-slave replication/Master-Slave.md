# Master/Slave 主从复制，读写分离

```
info replication //复制信息
slaveof ip port //配置主从机

```

学习的过程应不断破坏，不断修复。究根揭底，为什么

## 是什么：

主从复制，主机数据更新后更具配置和策略，自动同步到备机的master/slaver机制，Master以写为主，Slave以读为主

## 能干什么

读写分离

容灾恢复

## 怎么用

配从(库)不配主(库)

从库配置：slaveof 主库ip 主库端口

> 每次与master断开以后，都需要重新连接，除非配进redis.conf文件
>
> Info repolication //查看级别

#### 修改配置文件细节操作：

拷贝多个配置文件

开启daemonze yes

pid 文件名字

指定端口

Log文件名字

dump.rdb 名字

### 常用三招

一主二仆

![image-20200604113034085](D:\Boke\Databases\Nosql\Redis\Advanced\Publish and subscribe\a.png)

> init 
>
> 一个master两个slave
>
> 日志查看
>
> <img src="D:\Boke\Databases\Nosql\Redis\Advanced\Publish and subscribe\b.png" alt="image-20200604112939258" style="zoom:150%;" />
>
> 主从问题演示

> 主机写，从机读。
>
> 主机死亡，从机原地待命。主机从新上线，从机继续工作
>
> 从机死亡，若没有配置，则为新的master，可用slaveof 重连

#### 薪火相传：

> - 上一个slave可以是下一个slave的Master，Slave同样可以接收其他slaves的连接和同步请求，那么该slave作为了链条中下一个的master，可以有效减轻master写的压力
>
> - 中途变更向：会清除之前的数据，重新建立拷贝最新的
> - Slaveof 新主库IP 新主库端口

反客为主

> slaveeof no one 使当前数据库停止与其他数据库的同步，转成主数据库



## 复制原理：

> - slave启动成功连接到master后会发送一个sync命令
>
> - Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，master将传送整个数据文件到slave，以完成一次完全同步
> - 全量复制：而salve服务在接收到数据库文件数据后，将器存盘并加载到内存中
> - 增量复制：Master继续将新的所有收集到的修改命令依次传给slave，完成同步
> - 但是只要是重新连接master，一次完全同步（全量复制）将被自动执行

## 哨兵模式:

### 是什么？

> 反客为主自动版，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库

> 怎么使用：
>
> - 调整结构，6379带着80，81
>
> - 自定义的/myredis目录下新建sentinel.conf文件，名字绝不能错
>
> - 设置哨兵，填写内容
>
>   - sentinel monitor 被监控数据库（自己起名字）
>
>   ```
>   sentinel monitor <master-name> <ip> <redis-port> <quorum>
>   sentinel monitor host6379 127.0.0.1 6379 1
>   ```
>
>   
>
>   - 上面最后一个数字1，表示主机挂掉后salve投票看让谁接替成为主机，得票数多
>
> - 启动哨兵
>
> ```
> redis-sentinel ./sentinel.conf
> ```
>
> ![sentinel1](D:\Boke\Databases\Nosql\Redis\Advanced\z-master-slave replication\sentinel1.png)
>
> - 正常主从演示
> - 原有的master挂了
> - 投票新选
>
> ![sentinel2](D:\Boke\Databases\Nosql\Redis\Advanced\z-master-slave replication\sentinel2.png)
>
> - 重新主从继续运行，info repolication
> - ![sentinel3](D:\Boke\Databases\Nosql\Redis\Advanced\z-master-slave replication\sentinel3.png)
> - 问题：如果之前的master请重启回来，会不会双master冲突？不会，根据哨兵日志显示6379退出重新加入即降级为salve，并不会与master冲突
>
> ![sentinel4](D:\Boke\Databases\Nosql\Redis\Advanced\z-master-slave replication\sentinel4.png)

> 一组sentinel能同时监控多个master

## 复制的缺点

> 复制的延时，
>
> 由于所有的写操作都是先在Master上操作，然后同步到slave上，所以从master同步到slave机器有一定的延时，当系统很繁忙的时候，延时问题会更加验证，slave机器数量的增加也会使这个问题更加严重

