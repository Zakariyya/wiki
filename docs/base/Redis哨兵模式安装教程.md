# redis单节点简单安装

## 安装编译依赖
```sh
# centos
yum install gcc
yum -y install tcl*

# ubuntu
apt-get install build-essential tcl
```
## 下载redis
```sh
curl -o ./redis-2.8.24.tar.gz http://download.redis.io/releases/redis-2.8.24.tar.gz
```
## 编译安装
```sh
make
make test  
```
> 出现报错：You need tcl 8.5 or newer in order to run the Redis test
正解:apt-get update   如果已update 请执行下列逻辑
解决方式:安装 tcl

```sh
wget http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz  
sudo tar xzvf tcl8.6.1-src.tar.gz  -C /usr/local/  
cd  /usr/local/tcl8.6.1/unix/  
sudo ./configure  
sudo make  
sudo make install
make install
```

## 配置
```sh
mkdir /etc/redis/
cd /etc/redis
sudo cp /data/redis/redis-2.8.24/redis.conf ./redis.conf
```    
## 启动（&后台启动）
```sh
redis-server /etc/redis/redis.conf &
```
 
## 关闭
```sh
redis-cli shutdown
```

# 哨兵模式
> 在redis单节点安装前3步的基础上，以三台Centeos 7(因为在openstack内，所以10.36.8段的是浮动ip)为例：

||||||
|10.36.8.3|   redis1 | sentinel1 |#主 |内部ip:192.168.0.9  |
|10.36.8.4|   redis2 | sentinel2 |#从 |内部ip:192.168.0.10 |
|10.36.8.7|          | sentinel3 |#   |内部ip:192.168.0.11|

## 配置redis.conf


### 创建配置文件
```sh
sudo mkdir /etc/redis/
cd /etc/redis
sudo cp /data/redis/redis-2.8.24/redis.conf ./redis.conf
``` 
###  修改redis.conf（只配置主从两台，主从配置不同）
```sh
vim ./redis.conf

#10.36.8.3   redis1  sentinel1   #主 内部ip:192.168.0.9
bind 192.168.0.9
requirepass hostsname

#10.36.8.4   redis2  sentinel2   #从 内部ip:192.168.0.10
bind 192.168.0.10
slaveof 192.168.0.9 6379
masterauth hostsname
requirepass hostsname
```
## 修改sentinel.conf
### 创建配置文件：
```sh
cd /etc/redis
sudo cp /data/redis/redis-2.8.24/sentinel.conf ./sentinel.conf
```
### 修改sentinel.conf（三台配置相同）
|||||
|sentinel| monitor    |mymaster| 192.168.0.9 6379 2 |
|sentinel| auth-pass  |mymaster| hostsname             |


## 主从启动&测试
### 启动
```sh
# 192.168.0.9
redis-server /etc/redis/redis.conf  &

# 192.168.0.10
redis-server /etc/redis/redis.conf
``` 
> 可以看到控制台中有打印主从建立联系，命令后加&可后台启动

### 测试 (在主节点写入，从节点可看到)
```sh
# 192.168.0.9
$ redis-cli -a hostsname -h 192.168.0.9  -p 6379
192.168.0.9:6379> set a 1
OK
192.168.0.9:6379> get a
"1"
192.168.0.9:6379> 

# 192.168.0.10
$ redis-cli -a hostsname -h 192.168.0.10 -p 6379
192.168.0.10:6379> keys *
1) "a"
192.168.0.10:6379> get a
"1"
192.168.0.10:6379> 
```

## 哨兵启动&测试


### 启动
```sh
redis-sentinel /etc/redis/sentinel.conf &
```
    
### 测试
> 在任意一台,查询主节点

```sh
$ redis-cli -a hostsname -h 192.168.0.10 -p 26379
192.168.0.10:26379> SENTINEL get-master-addr-by-name mymaster
1) "192.168.0.9"
2) "6379"
192.168.0.10:26379> 
```
