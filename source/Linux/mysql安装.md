## mysql安装

cd /usr/local/

**wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.22-linux-glibc2.12-x86_64.tar.gz**

ll -t 查看已经下载好了安装包`mysql-5.7.22-linux-glibc2.12-x86_64.tar.gz`

**解压     tar -zxvf mysql-5.7.22-linux-glibc2.12-x86_64.tar.gz       **

**复制     cp -r mysql-5.7.22-linux-glibc2.12-x86_64 /usr/local/mysql**

2、添加系统mysql组和mysql用户 

添加系统mysql组    ** groupadd mysql**

添加mysql用户 **useradd -r -g**** mysql mysql **（添加完成后可用`id mysql`查看）



1. 安装数据库

2. 修改当前目录拥有者为mysql用户 **chown -R mysql:mysql ./**

   

bin/mysqld --initialize --user=root --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data

如果出现错误:

```shell
./bin/mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory
```

请执行 ` yum install -y libaio` 后再执行上面命令

成功后输出如下内容,生成了临时密码(Zy=lxsEkr0gM)

```2018-08-27T12:05:38.545487Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-08-27T12:05:38.545487Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-08-27T12:05:39.600635Z 0 [Warning] InnoDB: New log files created, LSN=45790
2018-08-27T12:05:39.722897Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2018-08-27T12:05:39.786218Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 83c6e27b-a9f1-11e8-873a-00163e0caf40.
2018-08-27T12:05:39.788230Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2018-08-27T12:05:39.788701Z 1 [Note] A temporary password is generated for root@localhost: Zy=lxsEkr0gM
```



配置my.cnf 



**vim /etc/my.cnf ** 写入如下内容

``````
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
socket=/tmp/mysql.sock
#不区分大小写 (sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 这个简单来说就是sql语句是否严格)
lower_case_table_names = 1
log-error=/var/log/mysqld.log    #chown -R mysql:mysql /var/log/ 需要给他赋mysql权限
pid-file=/usr/local/mysql/data/mysqld.pid

``````

 添加开机启动 `cp /usr/local/mysql/support-files/mysql.server  /etc/init.d/mysqld  ` 



修改   vim /etc/init.d/mysqld   	

``` shell
 vim /etc/init.d/mysqld   	
 basedir=/usr/local/mysql
 datadir=/usr/local/mysql/data
```



启动mysql   

``` shell
service mysql start
```



登录修改密码 mysql -uroot -p 上面初始化时的密码

**如果出现错误 需要添加软连接  ln -s /usr/local/mysql/bin/mysql /usr/bin**

```shell
alter user 'root'@'localhost' identified by 'root';   
flush privileges;    #刷新权限
GRANT ALL PRIVILEGES ON *.* TO 'root1'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;   #授权新用户root1
```



使用客户端连接mysql,出现如下错误,

远程权限问题。

````
出现“服务器连接错误Host 'XXX' is not allowed to connect to this MySQL server”的错误,看上面设置权限
````





































