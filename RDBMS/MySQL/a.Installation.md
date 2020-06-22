# Installation：

我所使用的是centsos7，MySQL5.7.28！！！

## 下载：

### 官方下载地址:https://dev.mysql.com/downloads/

![install1](D:\Boke\Databases\RDBMS\MySQL\install1.png)

最新版与归档(历史版本)

## 一、卸载老版本

```
// 检测
rpm -qa | grep mariadb
sudo yum remove {}
```

## 二、安装

### 创建用户和组

```
useradd mysql {-s /sbin/nologin mysql}
```

### 创建相关目录

```
mkdir -p  /app/database/ //软件目录（官方推荐为usr/local）
mkdir -p  /data/3306/	 //数据目录
mkdir -p  /binlog/3306/  //	日志目录
```

### 设置权限

```
chown -R mysql.mysql /app/ /data/ /binlog/ 
-R 代表递归一次性将主/子目录设置权限
mysql.mysql 是组和成员信息，（新组未添加组默认添加与成员名一样的组）
```

### 解压展开目录

```
tar xf {}.tar.gz (/dir，解压到。/dir目录)
```

#### 软链接

```
ln -s {filename} {linkname} //cd linkname 相当于cd filename
```

### 环境变量

```
vim /etc/profile
在最后一行新增
export PATH=/app/database/mysql/bin:$PATH
以上是自己配的路径，添加环境变量，不一定会相同
:wq 保存并退出后
source /etc/profile //生效配置
```

### 检测安装

```
任意目录下 
mysql -V 	 //若正常输出mysql版本号则安装成功
```