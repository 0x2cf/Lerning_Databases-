# Installtion：(非 唯一方式)

基本的安装参考上都有，很多东西就不一一赘述了，说些没有的。

具体的可参考：https://www.runoob.com/redis/redis-install.html

> // 压缩(这不用，自己做个复习)
>
> tar -czvf filename.tar.（tyepeof）
>
> // 解压
>
> tar -zxvf filename.tar.（tyepeof）

## Windows：

下载地址：https://github.com/tporadowski/redis/releases

## Linux：

```
先安装gcc：yum -y install gcc-c++
```

这个参考上没有，没有gcc安装会报错！！！

```
// 下载redis，版本号可自由选择，我选择的是redis-5.0.5
sudo wget http://download.redis.io/releases/{版本压缩包}

// 解压

tar -zxvf {版本压缩包}
// 安装
cd {版本文件包}
// 安装的具体过程可用看看 README.md，博文很多，层次不齐，最可靠的还是官网给出的引导
make MALLOC=libc
//快捷配置环境变量
cd ./src 
make install
```

![摘要](D:\Boke\Databases\Nosql\Redis\Basic\摘要.png)

查看进程是否在后台启动

```
ps -ef|grep redis
```

推荐参考文章：https://www.cnblogs.com/mracale/p/11328327.html