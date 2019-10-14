## 安装RabbitMQ

* ### 安装`erlang`（通过yum源来安装）

  * #### 因为RabbitMQ是由erlang实现的，所以要先安装erlang再安装rabbitMQ

  * #### 下载命令：

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# wget http://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
    ````

  * #### 加入yum源：

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# rpm -Uvh erlang-solutions-1.0-1.noarch.rpm
    ````

  * #### 开始安装：

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# yum install erlang
    ````


* ### 安装`rabbitmq`

  * #### 下载地址命令：

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]#  wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.15/rabbitmq-server-3.6.15-1.el6.noarch.rpm
    ````

  * #### 安装命令：

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# yum install rabbitmq-server-3.6.15-1.el6.rpm
    ````


* ### 配置`rabbitmq`

  * #### 先查看服务状态

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz ebin]# systemctl status rabbitmq-server.service
    ● rabbitmq-server.service - LSB: Enable AMQP service provided by RabbitMQ broker
       Loaded: loaded (/etc/rc.d/init.d/rabbitmq-server; bad; vendor preset: disabled)
       Active: active (running) since Tue 2018-08-28 17:46:50 CST; 48min ago
         Docs: man:systemd-sysv-generator(8)
      Process: 4672 ExecStop=/etc/rc.d/init.d/rabbitmq-server stop (code=exited, status=0/SUCCESS)
      Process: 5036 ExecStart=/etc/rc.d/init.d/rabbitmq-server start (code=exited, status=0/SUCCESS)
       CGroup: /system.slice/rabbitmq-server.service
               ├─5252 /bin/sh /etc/rc.d/init.d/rabbitmq-server start
               ├─5259 /bin/bash -c ulimit -S -c 0 >/dev/null 2>&1 ; /usr/sbin/rabbitmq-server
               └─5261 /bin/sh /usr/sbin/rabbitmq-server

    Aug 28 17:46:46 iz2ze0zcgmybh1wlebrk0mz systemd[1]: Starting LSB: Enable AMQP service provided by RabbitMQ broker...
    Aug 28 17:46:46 iz2ze0zcgmybh1wlebrk0mz su[5128]: (to rabbitmq) root on none
    Aug 28 17:46:46 iz2ze0zcgmybh1wlebrk0mz su[5278]: (to rabbitmq) root on none
    Aug 28 17:46:46 iz2ze0zcgmybh1wlebrk0mz su[5279]: (to rabbitmq) root on none
    Aug 28 17:46:50 iz2ze0zcgmybh1wlebrk0mz rabbitmq-server[5036]: Starting rabbitmq-server: SUCCESS
    Aug 28 17:46:50 iz2ze0zcgmybh1wlebrk0mz rabbitmq-server[5036]: rabbitmq-server.
    Aug 28 17:46:50 iz2ze0zcgmybh1wlebrk0mz systemd[1]: Started LSB: Enable AMQP service provided by RabbitMQ broker.
    ````

  * #### 启动服务

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# service rabbitmq-server start
    Starting rabbitmq-server (via systemctl):                  [  OK  ]
    ````

    或者

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# /sbin/service rabbitmq-server start
    ````



* ### 配置可视化管理界面

  * #### 安装插件

    `````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# /sbin/rabbitmq-plugins enable rabbitmq_management
    `````

  * #### 重启服务

    `````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# service rabbitmq-server restart
    `````

  * #### 通过[http://ip:15672](http://ip:15672/) 使用默认账号密码`guest,guest `登陆web页面

  * #### 登录出现错误`User can only log in via localhost`

    * 原因

      ```
      rabbitmq从3.3.0开始禁止使用guest/guest权限通过除localhost外的访问
      ```

    * 解决方案

      ```
      1. 找到/rabbitmq_server-3.6.14/ebin下面的rabbit.app文件
      2. 将loopback_users里的<<”guest”>>删除
         结果：[{rabbit, [{loopback_users, []}]}].
      3. 重启服务 
         systemctl restart rabbitmq-server.service 
      ```


* ### 用户管理

  * #### 查看当前用户

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz ebin]# rabbitmqctl list_users
    Listing users
    admin	[administrator]
    guest	[administrator]
    ````

  * #### 创建用户：

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# rabbitmqctl add_user admin 123456
    ````

  * #### 赋予管理员权限

    ````shell
    [root@iz2ze0zcgmybh1wlebrk0mz local]# rabbitmqctl set_user_tags admin administrator
    ````

     或者直接在web管理界面创建用户并赋权限

