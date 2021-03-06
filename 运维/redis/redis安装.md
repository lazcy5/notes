直接启动

###### 安装

[redis官网]: https://redis.io/download

```python
$ wget http://download.redis.io/releases/redis-5.0.5.tar.gz
$ tar xzf redis-5.0.5.tar.gz
$ cd redis-5.0.5
$ make
```

###### 启动

```python
#加上`&`号使redis以后台程序方式运行  
./redis-server & 
```

###### 检测

```python
#检测后台进程是否存在  
ps -ef |grep redis  
  
#检测6379端口是否在监听  
netstat -lntp | grep 6379  
  
#使用`redis-cli`客户端检测连接是否正常  
./redis-cli  
127.0.0.1:6379> keys *  
(empty list or set)  
127.0.0.1:6379> set key "hello world"  
OK  
127.0.0.1:6379> get key  
"hello world" 
```

###### 停止

```python
#使用客户端  
127.0.0.1:6379> shutdown  
#因为Redis可以妥善处理SIGTERM信号，所以直接kill -9也是可以的  
kill -9 PID 
```

#### 通过指定配置文件启动

配置文件redis.conf在redis根目录下

###### 修改配置文件

```python
#修改daemonize为yes，即默认以后台程序方式运行（还记得前面手动使用&号强制后台运行吗）。  
daemonize no  
#可修改默认监听端口  
port 6379  
#修改生成默认日志文件位置  
logfile "/home/futeng/logs/redis.log"  
#配置持久化文件存放位置  
dir /home/futeng/data/redisData
```

###### 启动时指定配置文件

```python
redis-server ./redis.conf  
#如果更改了端口，使用`redis-cli`客户端连接时，也需要指定端口，例如：  
redis-cli -p 6380 
```

#### 使用Redis启动脚本设置开机自启动

###### 启动脚本

推荐在生产环境中使用启动脚本方式启动redis服务。启动脚本`redis_init_script`位于位于Redis的`/utils/`目录下.

```python
#大致浏览下该启动脚本，发现redis习惯性用监听的端口名作为配置文件等命名，我们后面也遵循这个约定。  
#redis服务器监听的端口  
REDISPORT=6379  
#服务端所处位置，在make install后默认存放与`/usr/local/bin/redis-server`，如果未make install则需要修改该路径，下同。  
EXEC=/usr/local/bin/redis-server  
#客户端位置  
CLIEXEC=/usr/local/bin/redis-cli  
#Redis的PID文件位置  
PIDFILE=/var/run/redis_${REDISPORT}.pid  
#配置文件位置，需要修改  
CONF="/etc/redis/${REDISPORT}.conf" 
```

###### 配置环境

1. 根据启动脚本要求，将修改好的配置文件以端口为名复制一份到指定目录。需使用root用户。

```python
mkdir /etc/redis  
cp redis.conf /etc/redis/6379.conf 
```

2. 将启动脚本复制到/etc/init.d目录下，本例将启动脚本命名为redisd（通常都以d结尾表示是后台自启动服务）。

```python
cp redis_init_script /etc/init.d/redisd 
```

3. 设置为开机自启动

**此处直接配置开启自启动chkconfig redisd on将报错误：service redisd does not support chkconfig**
参照[此篇文章](http://www.cnblogs.com/goodspeed/archive/2012/10/18/2729615.html)，在启动脚本开头添加如下两行注释以修改其运行级别：

```python
#!/bin/sh  
# chkconfig:   2345 90 10  
# description:  Redis is a persistent key-value database  
#  再设置即可成功
```

```python
#设置为开机自启动服务器  
chkconfig redisd on  
#打开服务  
service redisd start  
#关闭服务  
service redisd stop  
```

