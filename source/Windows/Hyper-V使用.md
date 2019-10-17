## Hyper-V使用

### 1.找Hyper-V功能

![启动功能](..\images\hyper001.png)

### 2.开启服务

![开启服务](..\images\hyper002.png)

点击确定按钮后，系统提示重启，重新启动系统

### 3.安装虚拟机

#### 1.启动应用服务

重启系统后，在应用程序下查找Hyper-V服务

![服务查找](..\images\hyper003.png)

#### 2.新建虚拟机

![3-1](..\images\hyper003-1.png)

#### 3.自定义名称及存放路径

![4](..\images\hyper004.png)

#### 4.选择代

* 请选择[第一代]，否则后面安装CentOS会出错

![5](..\images\hyper005.png)

#### 5.分配内存

* 根据自身机器情况分配内存

![6](..\images\hyper006.png)

#### 6.配置网络 

* 先不配置，默认即可

![7](..\images\hyper007.png)

#### 6.虚拟硬盘配置

* 选择创建虚拟硬盘，点击下一步

![8](..\images\hyper008.png)

#### 7.安装选项配置

* 选择第二个，设置下载好的iso镜像

![9](..\images\hyper009.png)

#### 8.点击完成

![10](..\images\hyper010.png)

#### 9.启动虚拟机

![11](..\images\hyper011.png)

### 4.安装CentOS系统

* 请参考另一篇文章[虚拟机安装](../Linux/虚拟机安装.md#3安装centos7)中的虚拟机安装设置部分
* ...
* 等待安装完成

### 5.网络设置

#### 1.首先创建个虚拟交换机

![12](..\images\hyper012.png)

#### 2.选择外部，点击创建

![13](..\images\hyper013.png)

#### 3.选择你要连接的网卡，一般选择能上网的真实网卡

![14](..\images\hyper014.png)

#### 4.点击设置

![15](..\images\hyper015.png)

#### 5.选择刚刚创建的网络虚拟交换机

![16](..\images\hyper016.png)

#### 6.重启下centos,然后编辑你的第一个网卡（一般情况）

```shell
[root@localhost ~]# cd /etc/sysconfig/network-scripts/
[root@localhost network-scripts]# vi ifcfg-eth0 
```

#### 7.再配置里面这个改成yes

![17](..\images\hyper017.png)

#### 8.然后重启网络

```shell
[root@localhost network-scripts]# service network restart
Restarting network (via systemctl):                        [  ok  ]
```

#### 9.查看下ip,已经自动分配ip了

![18](..\images\hyper018.png)

#### 10.测试一下访问百度也可以访问了

![19](..\images\hyper019.png)


