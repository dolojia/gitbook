## Jenkins部署

### 准备Jenkins版本 （jenkins-2.32.3-1.1.noarch.rpm）

### 执行安装：rpm -ih jenkins-2.32.3-1.1.noarch.rpm

成功如下：

![img](./images/clip_image002.jpg)

### 自动安装完成之后

/usr/lib/jenkins/jenkins.war    WAR包 

vim /etc/sysconfig/jenkins       配置文件   修改端口为9001

/var/lib/jenkins/        默认的JENKINS_HOME目录

/var/log/jenkins/jenkins.log    Jenkins日志文件

```shell
/var/lib/jenkins/secrets/initialAdminPassword  首次登陆密码
```

### 安装完成之后启动jenkins

```shell
service jenkins start
```

###  跳过插件安装，待后续

### 设置用户名密码

admin df123456      131     admin   Sz123456

### 安装插件

Deploy to [**Container**](http://lib.csdn.net/base/docker) Plugin （这个是支持将代码部署到tomcat容器的）

GIT plugin

[Maven Integration plugin](https://plugins.jenkins.io/maven-plugin)  maven 项目

[**Git**](http://lib.csdn.net/base/git) Parameter 参数化构建git项目，可选分支或tag构建（可用于定义备份tag，失败时回滚）

### 设置jdk、maven、git



# Global Tool Configuration

Maven Configuration  默认

### 安装JDK

![img](./images/clip_image003.png)

### 安装maven

![img](./images/clip_image005.jpg)

### Git 安装

![img](./images/clip_image007.jpg)

 

 

### 创建maven项目

#### Git 源码设置(chmod -R 777 /var/lib/jenkins)

 

#### 设置git key免密登录

* cp ~/.ssh/id_rsa /var/lib/jenkins/.ssh/

* cp ~/.ssh/id_rsa.pub /var/lib/jenkins/.ssh/

   //或者Cp ssh to /var/lib/jenkins/.ssh

 ####  设置权限

```shell
chown jenkins id_rsa.pub 
chown jenkins id_rsa
chmod 400 id_rsa
```

#### 如果构建报错权限问题

#### 修改Jenkins用户权限为root

```shell
vim /etc/passwd
```

修改后

jenkins:x:0:486:Jenkins Continuous Integration Server:/var/lib/jenkins:/bin/false

####  配置Tomcat user.xml文件

```shell
 <role rolename="role1"/>
         <role rolename="manager"/>
         <role rolename="manager-gui"/>
		 <role rolename="manager-script"/>
		 <role rolename="manager-jmx"/>
		 <role rolename="manager-status"/>
		 <role rolename="admin-gui"/>
		 <role rolename="admin-script"/>
		 <user username="tomcat" password="tomcat" roles="tomcat"/>
		 <user username="both" password="tomcat" roles="tomcat,role1"/>
		 <user username="sa" password="admin" roles="tomcat,role1,manager,manager-gui,manager-script,manager-jmx,manager-status,admin-gui,admin-script"/>
		<user username="role1" password="tomcat" roles="role1"/>
```



**Jenkins执行远程Linux系统的shell命令** 

搜索并找到 [SSH Slaves plugin](https://wiki.jenkins-ci.org/display/JENKINS/SSH+Slaves+plugin) 插件

删除

//服务

sudo apt-get remove jenkins

//安装包，注意这里如果不是ubuntu那就yum

sudo apt-get remove --auto-remove jenkins

//配置和数据

sudo apt-get purge jenkins

sudo apt-get purge --auto-remove jenkins

![img](./images/clip_image009.jpg)