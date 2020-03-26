# Ubuntu ali

## edit source list
```sh
vim /etc/apt/sources.list
```
## delete all & replace
### ali
```sh
# deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricted
deb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties
deb http://archive.canonical.com/ubuntu xenial partner
deb-src http://archive.canonical.com/ubuntu xenial partner
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse
```

### 163 别用这个，mysql装不上
```
#163
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse

```

## update
```sh
sudo apt update

sudo apt upgrade
```

## 关于Ubuntu中Could not get lock /var/lib/dpkg/lock解决方案
```
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```

### 找到并且杀掉所有的apt-get 和apt进程

> 运行下面的命令来生成所有含有 apt 的进程列表，你可以使用ps和grep命令并用管道组合来得到含有apt或者apt-get的进程。

```
ps -A | grep apt
```


找出所有的 apt 以及 apt-get 进程
```

sudo kill -9 processnumber

或者
sudo kill -SIGKILL processnumber

```

***
比如，下面命令中的
 9
是 
 SIGKILL
的信号数，它会杀掉第一个 apt 进程


```

sudo kill -9 进程ID

或者
sudo kill -SIGKILL  进程ID
```

### 删除锁定文件

锁定的文件会阻止 Linux 系统中某些文件或者数据的访问，这个概念也存在于 Windows 或者其他的操作系统中。

一旦你运行了 **apt-get** 或者 **apt** 命令，锁定文件将会创建于 **/var/lib/apt/lists/、/var/lib/dpkg/、/var/cache/apt/archives/** 中。

这有助于运行中的 **apt-get** 或者 **apt** 进程能够避免被其它需要使用相同文件的用户或者系统进程所打断。当该进程执行完毕后，锁定文件将会删除。

当你没有看到 **apt-get** 或者 **apt** 进程的情况下在上面两个不同的文件夹中看到了锁定文件，这是因为进程由于某个原因被杀掉了，因此你需要删除锁定文件来避免该错误。

首先运行下面的命令来移除 **/var/lib/dpkg/** 文件夹下的锁定文件：
```
sudo rm /var/lib/dpkg/lock
```
之后像下面这样强制重新配置软件包：
```
sudo dpkg --configure -a
```
也可以删除 **/var/lib/apt/lists/** 以及缓存文件夹下的锁定文件：
```
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
```
接下来，更新你的软件包源列表：
```
sudo apt update

或者
sudo apt-get update
```
> 总结一下，对于 Ubuntu（以及它的衍生版）用户在使用 apt-get 或者 apt 也叫 aptitude 命令[7]时遇到的问题，我们已经用两种方法来解决了。

***
> 参考： [关于Ubuntu中Could not get lock /var/lib/dpkg/lock解决方案](https://blog.csdn.net/u011596455/article/details/60322568)


