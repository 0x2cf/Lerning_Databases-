# BASIC CONFIG

我所使用的是centsos7，5.7.28！！！

## 初始化系统库表

```
mysqld --initialize-insecure --user=mysql --basedir=/app/database/mysql --datadir=/data/3306/
//5.6版本初始化
//mysqlscripts/mysql_install_db --user=mysql --basedir=/app/database/mysql --datadir=/data/3306/
```

#### 报错处理：

```
ERROR：mysqld: error while loading shared libraries:libaio.so.l:
cannot open shared object file:No such file or directory

// 缺少依赖软件包，下载依赖软件包即可
yum install libaio-devel
再次运行，即可：
mysqld --initialize-insecure --user=mysql --basedir=/app/database/mysql --datadir=/data/3306/

常出现的错误：
情景1：由于第一次并没有并没有初始化成功，错误提示文件已存在！
解决：删除对应目录下的文件，重新初始化即可
rm -rf /data/3306/*
```

## 基础配置文件设置：

```
cat > /etc/my.cnf << EOF
[mysqld]
user=mysql
basedir=/app/database/mysql
datadir=/data/3306
server_id=6
prot=3306
socket=/tmp/mysql.sock
[mysql]
socket=/tmp/mysql.sock
EOF
```

## 准备MySQL启动脚本：

```
//进入启动脚本所在目录
cd /app/database/mysql/support-files/
//便捷设置，将启动脚本拷贝到系统软件管理目录中(为了方便调用)
cp mysql.server /etc/init.d/mysqld
```

