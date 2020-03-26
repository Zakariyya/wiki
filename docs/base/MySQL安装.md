# mysql安装文档

## 安装
```sh
apt-get install software-properties-common
add-apt-repository 'deb http://archive.ubuntu.com/ubuntu trusty universe'
sudo apt-get update
sudo apt install mysql-server
```
## 使用service 启动、关闭MySQL服务

```sh
service mysql start
service mysql stop
service mysql restart
使用ps -aux | grep mysql 查询是否启动成功
```

## 修改root权限
```sh
mysql -u root -p 
show databases;
use mysql;
```
### 两种

1. 改表法 
可能是你的帐号不允许从远程登陆，只能在localhost。这个时候只要在localhost的那台电脑，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%" 
```sh
mysql -u root -p 
use mysql; 
update user set host = '%' where user = 'root'; 
select host, user from user;
```
1. 授权法
赋予任何主机访问数据的权限 :
```sh
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'WITH GRANT OPTION 
```
修改生效 :mysql>FLUSH PRIVILEGES 

结果出现，就OK了：
```sh
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| %         | root             |
| localhost | debian-sys-maint |
| localhost | mysql.session    |
| localhost | mysql.sys        |
+-----------+------------------+
```

## 无法远程连接MySQL服务器 ( 直接跳到最后标题 )

要在Linux终端中测试远程MySQL连接：
```sh
mysql -u username -h 192.16.8.1.22 -p
```
或者通过WIndows的Telnet：
```sh
telnet 192.16.8.1.22 3306
```
如果在尝试连接到远程MySQL服务器时遇到错误
```sh
ERROR 2003 (HY000): Can't connect to MySQL server on '192.16.8.1.22' (111)
```
或者在Telnet中
```sh
Connecting To 192.16.8.1.22...Could not open connection to the host, on port 3306: Connect failed
```
### 检查防火墙
首先，如果ufw启用了防火墙，请确保您有MySQL规则
```sh
sudo ufw allow mysql
```
重启服务
```sh
sudo service ufw restart
```

### 检查MySQL配置(百试百灵，终极)

bind-address在MySQL配置文件 mysqld.cnf 中 ，把这鬼东西注释掉
```sh
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
找到行**bind-address** 
```
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
# bind-address = 127.0.0.1 (把这行注释掉)
```
以上行告诉MySQL只接受本地连接。通过#在它之前添加来注释掉这一行。

重新启动MySQL服务以使更改生效。
```sh
sudo service mysql restart
```
***
参考 ：[Can’t connect to MySQL server remotely on Ubuntu](https://devanswers.co/cant-connect-mysql-server-remotely/#comment-1727)








