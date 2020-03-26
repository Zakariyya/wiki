# nginx安装及配置
## nginx安装
- 安装gcc g++的依赖库
	- ubuntu平台可以使用如下命令。
		```
		apt install build-essential libtool make gcc 
		```
	- centeros平台可以使用如下命令。
		centos平台编译环境使用如下指令
		安装make：
		```
		yum -y install gcc automake autoconf libtool make
		```
		安装g++:
		```
		yum install gcc gcc-c++　
		```
- 安装 pcre依赖库（http://www.pcre.org/）
	```
	sudo apt update
	sudo apt install libpcre3 libpcre3-dev
	```
- 安装 zlib依赖库（http://www.zlib.net）
	```
	apt install zlib1g-dev
	```
- 安装 ssl依赖库
	```
	apt install openssl
	```
- 下载最新版本 Nginx：
	```
	wget http://nginx.org/download/nginx-1.11.3.tar.gz
	```
- 解压：
	```
	tar -zxvf nginx-1.11.3.tar.gz
	```
- 进入解压目录：
	```
	cd nginx-1.11.3
	```
- 配置：
	```
	./configure --prefix=/usr/local/nginx 
	```
- 编辑nginx：
	```
	make
	```
  > 注意：这里可能会报错，提示“pcre.h No such file or directory”,具体详见：[http://stackoverflow.com/questions/22555561/error-building-fatal-error-pcre-h-no-such-file-or-directory](http://stackoverflow.com/questions/22555561/error-building-fatal-error-pcre-h-no-such-file-or-directory)
	
	需要安装 
	```
	libpcre3-dev,命令为：sudo apt install libpcre3-dev
	```
- 安装nginx：
	```
	sudo make install
	```
- 编辑/usr/local/nginx/conf/nginx.conf，看 “nginx配置” 
- 启动nginx：
	```
	sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
	```
	> 注意：-c 指定配置文件的路径，不加的话，nginx会自动加载默认路径的配置文件，可以通过 -h查看帮助命令。
- 查看nginx进程：
	```
	ps -ef|grep nginx
	```

## nginx配置
```
vim /usr/local/nginx/conf/nginx.conf


server {
	listen	8090;
	server_name  localhost;
	proxy_redirect	off;
	proxy_set_header	X-Real-IP $remote_addr;
	proxy_set_header  Host $host:$server_port;
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	location /cloud {
		proxy_pass http:baidu.com:8096/cloud;
	}
	location /cloudweb {
		proxy_pass http:baidu.com:8099/cloudweb/;
	}
	location /cloudrest {
		proxy_pass http:baidu.com:8098/cloudrest;
	}
	location / {
		proxy_pass http:baidu.com:8088;
	}
	location /authweb {
		proxy_pass http:baidu.com:8084/authweb/;
	}
	location /authrest {
		proxy_pass http:baidu.com:8083/authrest/;
	}
	error_page   500 502 503 504  /50x.html;
	location = /50x.html {
		root   html;
	}
}
```