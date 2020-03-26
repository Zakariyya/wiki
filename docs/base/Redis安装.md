# 安装redis 

## 安装准备 
> 确保机子能执行相关命令

```
apt install make gcc 
```
## 获取redis资源
```wget http://download.redis.io/releases/redis-4.0.8.tar.gz```
## 解压
```tar xzvf redis-4.0.8.tar.gz```
## 安装
```sh
cd redis-4.0.8
make
make install
cd src
make install PREFIX=/usr/local/redis
```
### 出现以下错误（假如出现该情况，不是每次）
```sh
zmalloc.h:50:31: error: jemalloc/jemalloc.h: No such file or directory
zmalloc.h:55:2: error: #error "Newer version of jemalloc required"
make[1]: *** [adlist.o] Error 1
make[1]: Leaving directory `/data0/src/redis-2.6.2/src'
make: *** [all] Error 2
```
####解决办法：
```sh
make MALLOC=libc
```

## 移动配置文件到安装目录下
```sh
cd ..
mkdir /usr/local/redis/etc
```
### 修改redis启动配置文件 
#### 注释以下绑定的主机地址
```
vim /opt/redis-4.0.8/redis.conf

# bind 127.0.0.1
```
#### redis.conf 修改redis的保护模式为no，不启用
```
127.0.0.1:6379> config set protected-mode "no" 

或者 

修改 redis.conf 配置文件的

protected-mode值为 no 
daemonize 值为 yes ## 后台启动

cp redis.conf /usr/local/redis/etc
```

- 启动（&后台启动）：
```redis-server /usr/local/redis/etc/redis.conf  &```
- 关闭：
```redis-cli shutdown```


## redis配置密码

- 通过配置文件进行配置
redis配置文件通常在/usr/local/redis/etc/redis.conf中，打开配置文件找到
```
#requirepass foobared
```
- 去掉行前的注释，并修改密码为所需的密码,保存文件
```
requirepass 123456
```
- 重启redis

## 查看redis进程
```
ps -aux | grep redis
```