# Publish and subscribe：

是什么？

进程间的一种信息通信模式： 发送者（pub）发送消息，订阅者（sub）接收消息

订阅/发布图：![Pub and sub1](D:\Boke\Databases\Nosql\Redis\Advanced\Publish and subscribe\Pub and sub1.png)

## 命令：

> psubscribf pattern  //订阅一个或多个符合模式频道
>
> pubsub subcommand 【argument】 // 查看订阅 与发布系统状态
>
> punsubscribe pattern 退订
>
> subscribe channel 订阅给顶的一个或多个频道信息
>
> UNSUBscribe channel // 指退订给定的频道

案例

![Pub and sub2](D:\Boke\Databases\Nosql\Redis\Advanced\Publish and subscribe\Pub and sub2.png)

