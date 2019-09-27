## 安装MongoDB

* #### Configure the package management system (`yum`)

  * #### Create a `/etc/yum.repos.d/mongodb-org-4.0.repo` file so that you can install MongoDB directly using `yum`:

    ````shell
    [mongodb-org-4.0]
    name=MongoDB Repository
    baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
    gpgcheck=1
    enabled=1
    gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
    ````

  * #### Install the MongoDB packages.

  * 安装MongoDB的最新的稳定版本,发出以下命令

    ````shell
    sudo yum install -y mongodb-org
    ````

  * 安装MongoDB的特定版本,指定每个组件分别包和包名称添加版本号,如以下示例

    ````shell
    sudo yum install -y mongodb-org-4.0.2 mongodb-org-server-4.0.2 mongodb-org-shell-4.0.2 mongodb-org-mongos-4.0.2 mongodb-org-tools-4.0.2
    ````


  * 出现错误：curl: (35) SSL connect error

    * 原因：nss版本太旧，升级解决

      ````shell
      [root@IDCWX16 mongodb]# yum -y update nss
      ````

* ## Run MongoDB Community Edition（运行MongoDB社区版 ）

  * ####  Start MongoDB：

    ````shell
    sudo service mongod start
    ````

  * ####  Stop MongoDB：

    ```shell
    sudo service mongod stop
    ```

  *  ##### Restart MongoDB

    ````shell
    sudo service mongod restart
    ````

  * #### Begin using MongoDB

    ````shell
    mongo --host 127.0.0.1:27017
    ````

  * 