# 安装JDK1.8
		
1. 用浏览器打开 [https://www.oracle.com/downloads/index.html](https://www.oracle.com/downloads/index.html)
1. 找到 Downloads by Category\*   点击Java  再点击 Java (JDK) for Developers
1. 找到 Java SE 8u181 点击 JDK DOWNLOAD  找到 Java SE Development Kit 8u181 点击 Accept License Agreement
1. 选择 Linux x64 jdk-8u181-linux-x64.tar.gz 点击名称下载，将下载的压缩包上传到虚拟机(或者复制下载地址 使用wget命令在虚拟机上下载 使用wget下载需要重命名下载文件 jdk-8u181-linux-x64.tar.gz )
```
wget http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.tar.gz
```
1. ```tar -xvf jdk-8u181-linux-x64.tar.gz``` 解压安装包
1. ```vim /etc/profile```
1. ```
	文末加上

	### JAVA_HOME
	export JAVA_HOME=/opt/jdk
	export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
```
1. 最后使用 ```source /etc/profile``` 让 profile文件立即生效。
1. 使用 ```javac``` 及 ```java -version``` 命令来检查是否按照成功
