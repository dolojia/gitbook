# Zookeeper 安装

在安装ZooKeeper之前，请确保你的系统是在以下任一操作系统上运行：

- `**任意Linux OS** - 支持开发和部署。适合演示应用程序。`
- `**Windows OS** - 仅支持开发。`
- `**Mac OS** - 仅支持开发。`

ZooKeeper服务器是用Java创建的，它在JVM上运行。你需要使用JDK 6或更高版本。

现在，按照以下步骤在你的机器上安装ZooKeeper框架。

## 步骤1：验证Java安装

相信你已经在系统上安装了Java环境。现在只需使用以下命令验证它。

```
$ java -version
```

如果你在机器上安装了Java，那么可以看到已安装的Java的版本。否则，请按照以下简单步骤安装最新版本的Java。

### 步骤1.1：下载JDK

通过访问链接下载最新版本的JDK，并下载最新版本的Java。

最新版本（在编写本教程时）是JDK 8u 60，文件是“jdk-8u60-linuxx64.tar.gz"。请在你的机器上下载该文件。

### 步骤1.2：提取文件

通常，文件会下载到download文件夹中。验证并使用以下命令提取tar设置。

```
$ cd /go/to/download/path
$ tar -zxf jdk-8u60-linux-x64.gz
```

### 步骤1.3：移动到opt目录

要使Java对所有用户可用，请将提取的Java内容移动到“/usr/local/java"文件夹。

```
$ su 
password: (type password of root user)
$ mkdir /opt/jdk
$ mv jdk-1.8.0_60 /opt/jdk/
```

### 步骤1.4：设置路径

要设置路径和JAVA_HOME变量，请将以下命令添加到〜/.bashrc文件中。

```
export JAVA_HOME = /usr/jdk/jdk-1.8.0_60
export PATH=$PATH:$JAVA_HOME/bin
```

现在，将所有更改应用到当前运行的系统中。

```
$ source ~/.bashrc
```

### 步骤1.5：Java替代

使用以下命令更改Java替代项。

```
update-alternatives --install /usr/bin/java java /opt/jdk/jdk1.8.0_60/bin/java 100
```

### 步骤1.6

使用步骤1中说明的验证命令(java -version)验证Java安装。

## 步骤2：ZooKeeper框架安装

### 步骤2.1：下载ZooKeeper

要在你的计算机上安装ZooKeeper框架，请访问以下链接并下载最新版本的ZooKeeper。http://zookeeper.apache.org/releases.html

到目前为止，最新版本的ZooKeeper是3.4.6(ZooKeeper-3.4.6.tar.gz)。

### 步骤2.2：提取tar文件

使用以下命令提取tar文件

```
$ cd opt/
$ tar -zxf zookeeper-3.4.6.tar.gz
$ cd zookeeper-3.4.6
$ mkdir data
```

### 步骤2.3：创建配置文件

使用命令 vi conf/zoo.cfg 和所有以下参数设置为起点，打开名为 conf/zoo.cfg 的配置文件。

```
$ vi conf/zoo.cfg

tickTime = 2000
dataDir = /path/to/zookeeper/data
clientPort = 2181
initLimit = 5
syncLimit = 2
```

一旦成功保存配置文件，再次返回终端。你现在可以启动zookeeper服务器。

### 步骤2.4：启动ZooKeeper服务器

执行以下命令

```
$ bin/zkServer.sh start
```

执行此命令后，你将收到以下响应

```
$ JMX enabled by default
$ Using config: /Users/../zookeeper-3.4.6/bin/../conf/zoo.cfg
$ Starting zookeeper ... STARTED
```

### 步骤2.5：启动CLI

键入以下命令

```
$ bin/zkCli.sh
```

键入上述命令后，将连接到ZooKeeper服务器，你应该得到以下响应。

```
Connecting to localhost:2181
................
................
................
Welcome to ZooKeeper!
................
................
WATCHER::
WatchedEvent state:SyncConnected type: None path:null
[zk: localhost:2181(CONNECTED) 0]
```

### 停止ZooKeeper服务器

连接服务器并执行所有操作后，可以使用以下命令停止zookeeper服务器。

```
$ bin/zkServer.sh stop
```



客户端连接报错如下：

```java
Opening socket connection to server localhost.dolojia/192.168.112.131:2181. Will not attempt to authenticate using SASL (unknown error)
```

是因为防火墙的原因

在CentOS 7或RHEL 7中防火墙由firewalld来管理

```shell
firewall-cmd --zone=public --add-port=8088/tcp --permanent #（--permanent永久生效，没有此参数重启后失效）
firewall-cmd --zone=public --remove-port=80/tcp --permanent #删除

systemctl start firewalld.service                               #查看防火墙状态 
systemctl stop firewalld.service                            #完全关闭防火墙
firewall-cmd --permanent --zone=public --add-port=2181/tcp  #打开2181端口


``
```

