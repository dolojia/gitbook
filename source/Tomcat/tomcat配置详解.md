## tomcat中server.xml配置详解

## 元素结构

<Server>
​    <Listener />
​    <GlobaNamingResources>
​    </GlobaNamingResources
​    <Service>
​        <Connector />
​        <Engine>
​            <Logger />
​            <Realm />
​               <host>
​                   <Logger />
​                   <Context />
​               </host>
​        </Engine>
​    </Service>
</Server>

###元素节点介绍

| 元素名                                                       | 属性                                                         |                             解释                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ | :----------------------------------------------------------: |
| server                                                       | port                                                         |        指定一个端口，这个端口负责监听关闭tomcat的请求        |
| shutdown                                                     | 指定向端口发送的命令字符串                                   |                                                              |
| service                                                      | name                                                         |                      指定service的名字                       |
| Connector(表示客户端和service之间的连接)                     | port                                                         | 指定服务器端要创建的端口号，并在这个断口监听来自客户端的请求 |
| minProcessors                                                | 服务器启动时创建的处理请求的线程数                           |                                                              |
| maxProcessors                                                | 最大可以创建的处理请求的线程数                               |                                                              |
| minSpareThreads                                              | 该Connector先创建5个线程等待客户请求，每个请求由一个线程负责 |                                                              |
| maxSpareThread                                               | 设定在监听端口的线程的最大数目,这个值也决定了服务器可以同时响应客户请求的最大数目.默认值为200 |                                                              |
| enableLookups                                                | 如果为true，则可以通过调用request.getRemoteHost()进行DNS查询来得到远程客户端的实际主机名，若为false则不进行DNS查询，而是返回其ip地址 |                                                              |
| redirectPort                                                 | 指定服务器正在处理http请求时收到了一个SSL传输请求后重定向的端口号 |                                                              |
| acceptCount                                                  | 指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理 |                                                              |
| connectionTimeout                                            | 指定超时的时间数(以毫秒为单位)                               |                                                              |
| Engine(表示指定service中的请求处理机，接收和处理来自Connector的请求) | defaultHost                                                  | 指定缺省的处理请求的主机名，它至少与其中的一个host元素的name属性值是一样的 |
| Context(表示一个web应用程序，通常为WAR文件，关于WAR的具体信息见servlet规范) | docBase                                                      |            应用程序的路径或者是WAR文件存放的路径             |
| path                                                         | 表示此web应用程序的url的前缀，这样请求的url为http://localhost:8080/path/**** |                                                              |
| reloadable                                                   | 这个属性非常重要，如果为true，则tomcat会自动检测应用程序的/WEB-INF/lib 和/WEB-INF/classes目录的变化，自动装载新的应用程序，我们可以在不重起tomcat的情况下改变应用程序 |                                                              |
| host(表示一个虚拟主机)                                       | name                                                         |                          指定主机名                          |
| appBase                                                      | 应用程序基本目录，即存放应用程序的目录                       |                                                              |
| unpackWARs                                                   | 如果为true，则tomcat会自动将WAR文件解压，否则不解压，直接从WAR文件中运行应用程序 |                                                              |
| Logger(表示日志，调试和错误信息)                             | className                                                    | 指定logger使用的类名，此类必须实现org.apache.catalina.Logger 接口 |
| prefix                                                       | 指定log文件的前缀                                            |                                                              |
| suffix                                                       | 指定log文件的后缀                                            |                                                              |
| timestamp                                                    | 如果为true，则log文件名中要加入时间，如下例:localhost_log.001-10-04.txt |                                                              |
| Realm(表示存放用户名，密码及role的[数据库](http://lib.csdn.net/base/14)) | className                                                    | 指定Realm使用的类名，此类必须实现org.apache.catalina.Realm接口 |
| Valve(功能与Logger差不多，其prefix和suffix属性解释和Logger 中的一样) | className                                                    | 指定Valve使用的类名，如用org.apache.catalina.valves.AccessLogValve类可以记录应用程序的访问信息 |
| directory                                                    | 指定log文件存放的位置                                        |                                                              |
| pattern                                                      | 有两个值，common方式记录远程主机名或ip地址，用户名，日期，第一行请求的字符串，HTTP响应代码，发送的字节数。combined方式比common方式记录的值更多 |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |

























* [下载地址](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

* 在usr/local下新建目录Java

  ```shell
  [root@iz2ze0zcgmybh1wlebrk0mz local]# mkdir java
  ```

* 将下载好的jdk-7u71-linux-x64.rpm文件拷贝到 Java目录中

* 上传完后的文件如下，文件只有读写权限，没有执行权限 

* 使用如下命令授权，如果文件已经有了执行权限，此步骤可省略

  ```shell
  # chmod 755 jdk-7u71-linux-x64.rpm
  ```

**2.安装JDK**

* 执行如下命令安装jdk

```shell
# rpm -ivh jdk-7u71-linux-x64.rpm
```

* 如果在安装时出现如下错误  

  warning:waiting for transaction lock on /var/lib/rpm/.rpm.lock 

* 使用如下命令来进行安装

  ```shell
  # sudo rpm -ivh jdk-7u71-linux-x64.rpm
  ```

* 如果仍然不可以，使用如下命令强制解锁后再次安装即可

  ```
  # sudo rm /var/lib/rpm/.rpm.lock
  ```

**3.配置环境变量**

* 使用 vim 编辑器打开文件/etc/profile

  ```shell
  # vim /etc/profile 
  ```

* 在文件尾部添加如下内容，保存退出

  ```shell
  export JAVA_HOME=/usr/java/jdk1.7.0_71
  export PATH=$JAVA_HOME/bin:$PATH
  export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tool.jar
  ```

* 此时，我们刚刚配置的环境变量并没有起效，输入如下命令，使用环境变量立即生效

  ```shell
  # source /etc/profile
  ```

* 输入如下命令验证环境变量是否生效

  ```shell
  # echo $PATH
  ```

* 输入如下命令查看jdk版本

  ```shell
  # java -version
  openjdk version "1.8.0_181"
  OpenJDK Runtime Environment (build 1.8.0_181-b13)
  OpenJDK 64-Bit Server VM (build 25.181-b13, mixed mode)
  ```

## yum安装JDK

**1.检查系统原版并卸载** 

输入如下命令查看系统已安装的jdk

```shell
# rpm -qa | grep java
# rpm -qa | grep jdk
```

如果已经安装了jdk，使用如下命令卸载，yum会自动检测，卸载删除jdk的相关安装包

```shell
# yum -y remove java*
# yum -y remove jdk*
```

**2.jdk安装**

查看java相关列表（jdk版本信息），笔者选择安装的是openjdk1.8

```shell
# yum list | grep jdk
```

**3.使用如下命令安装jdk**

```shell
# yum -y install java-1.8.0-openjdk.x86_64
```

**4. 等待安装完成，验证版本**

```shell
# java -version
openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-b13)
OpenJDK 64-Bit Server VM (build 25.181-b13, mixed mode)
```

