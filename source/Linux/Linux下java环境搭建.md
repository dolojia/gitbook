## Linux下java环境搭建

### rpm方式安装JDK

**1.下载JDK** 

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

