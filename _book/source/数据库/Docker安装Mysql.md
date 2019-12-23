## Docker安装Mysql

#### 搜索mysql镜像

```shell
docker search mysql
```

#### 下载mysql镜像

```shell
docker pull mysql
```

#### 启动mysql镜像(普通方式)

```shell
docker run -di --name dolo_mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql
```

>-p 代表端口映射，格式为 宿主机映射端口:容器运行端口
>
>-e 代表添加环境变量 MYSQL_ROOT_PASSWORD是root用户的登陆密码
>
>--name 修改别名

#### 启动mysql镜像(挂载宿主机目录方式)

>根据镜像说明可知：
>
>- 默认的配置文件是：/etc/mysql/my.cnf
>- 默认的数据目录是：/var/lib/mysql

```shell
docker run -di --name dolo_mysql -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql --restart=always -e MYSQL_ROOT_PASSWORD=root mysql --lower_case_table_names=1 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci 
```

> ```tex
> --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci 修改默认编码
> -v 将容器目录挂载至宿主机本地目录下
> --restart=always 无论退出状态是如何，都重启容器
> lower_case_table_names=1 忽略大小写
> ```

#### 进入容器

```shell
docker exec -it dolo_mysql /bin/bash
```

#### 登陆mysql

```shell
mysql -u root -p
```

#### Navicat 远程连接docker容器中的mysql 报错1251 - Client does not support authentication protocol 解决办法

##### 查看mysql版本信息

```shell
mysql> status;
--------------
mysql  Ver 8.0.18 for Linux on x86_64 (MySQL Community Server - GPL)
```

##### 进行授权远程连接(注意mysql 8.0跟之前的授权方式不同) 

```shell
#授权
mysql> GRANT ALL ON *.* TO 'root'@'%';
Query OK, 0 rows affected (0.09 sec)
#刷新权限
mysql> flush privileges;
Query OK, 0 rows affected (0.07 sec)
```

##### 因为Navicat只支持旧版本的加密,需要更改mysql的加密规则 

```shell
#更改加密规则
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;
Query OK, 0 rows affected (0.17 sec)
#更新root用户密码
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
Query OK, 0 rows affected (0.19 sec)
#刷新权限
mysql> flush privileges;
Query OK, 0 rows affected (0.07 sec)
```

##### 验证 Navicat连接测试

![](..\images\mysql001.png)