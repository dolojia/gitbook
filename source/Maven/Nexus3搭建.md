## Nexus3搭建

### 安装方式

#### 源码安装

```shell
wget http://download.sonatype.com/nexus/3/nexus-3.19.1-01-unix.tar.gz
tar -xvf nexus-3.19.1-01-unix.tar.gz
#修改端口及项目context-path
vim /data/local/software/nexus-3.19.1-01/etc/nexus-default.properties
/data/local/software/nexus-3.19.1-01/bin/nexus start
```

#### Docker下载安装

```shell
#查找Nexus镜像
[root@localhost home]# docker search Nexus
#下载镜像
[root@localhost home]# docker pull sonatype/nexus3
#启动容器Nexus默认端口为8081,设置NEXUS_CONTEXT=nexus
[root@localhost home]# docker run -d -p 8081:8081 sonatype/nexus3 -e NEXUS_CONTEXT=nexus 
```

启动成功访问：[http://ip:8081](http://ip:8081/)

![](E:\gitbook\source\images\nexus1-001.png)

Nexus默认配置信息

> 默认配置文件：/opt/sonatype/nexus/etc/nexus-default.properties

点击`Sign out`提示输入用密码

> 初始密码位置：/nexus-data/admin.password

根据提示修改密码

### 仓库分类介绍

> `hosted`：本地仓库，通常我们会部署自己的构件到这一类型的仓库。比如公司的第二方库。

> `proxy`：代理仓库，它们被用来代理远程的公共仓库，如maven中央仓库。

> `group`：仓库组，用来合并多个hosted/proxy仓库。

![](..\images\nexus3-007.jpg)

### 仓库创建

#### 创建私服本地仓库

![3-001](..\images\nexus3-001.png)

![3-001](..\images\nexus3-002.png)

![3-001](..\images\nexus3-003.png)

#### 创建阿里云代理仓库

![](..\images\nexus3-007.png)

#### 配置仓库组提供外部使用

![](..\images\nexus3-008.png)

### 上传单个jar文件至私服

#### 方式一：使用命令方式

>  配置setting.xml

```xml
<!--在settings.xml的<servers></servers>-->
<server>   
    <id>仓库id</id>
    <username>admin</username>
    <password>admin123</password>   
</server>
```

> 在cmd命令行中输入

```shell
mvn deploy:deploy-file -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.4      -Dpackaging=jar -Dfile=D:\develop\repo\com\oracle\ojdbc6\11.2.0.4\ojdbc6-11.2.0.4.jar         -Durl=http://ip:8081/repository/maven_repo/ -DrepositoryId=maven_repo
```

> 如果进行deploy时返回Return code is: 401错误，则需要进行用户验证或者你已经验证的信息有误

#### 方式二：界面操作

* 选择要上传的仓库

![3-001](..\images\nexus3-004.png)

* 填写信息点击`Upload`

![3-001](..\images\nexus3-006.png)

### 批量上传本地jar文件

* 上传文件至服务器

* 将本地jar文件传至服务器`/home/repo`目录

* 在目录下创建批量导入脚本`push_nexus.sh`

```shell
#!/bin/bash
# copy and run this script to the root of the repository directory containing files
# this script attempts to exclude uploading itself explicitly so the script name is important
# Get command line params
while getopts ":r:u:p:" opt; do
	case $opt in
		r) REPO_URL="$OPTARG"
		;;
		u) USERNAME="$OPTARG"
		;;
		p) PASSWORD="$OPTARG"
		;;
	esac
done
 
find . -type f -not -path './mavenimport\.sh*' -not -path '*/\.*' -not -path '*/\^archetype\-catalog\.xml*' -not -path '*/\^maven\-metadata\-local*\.xml' -not -path '*/\^maven\-metadata\-deployment*\.xml' | sed "s|^\./||" | xargs -I '{}' curl -u "$USERNAME:$PASSWORD" -X PUT -v -T {} ${REPO_URL}/{} ;
```

* 给脚本赋权限：`chmod -R 777 push_nexus.sh` 

```shell
# 执行批量上传脚本
# 用户名/密码：admin/admin123
# 上传仓库地址：http://ip:8081/repository/mavn_repo/
[root@localhost repo]# ./push_nexus.sh -u admin -p admin123 -r http://ip:8081/repository/mavn_repo/
```

* 查看是否导入成功

![007](..\images\nexus007.png)

![007](..\images\nexus008.png)

### POM文件配置

```xml
<!-- 配置远程发布到私服,mvn deploy,不要随意执行，统一管理维护 -->
    <distributionManagement>
        <repository>
            <!--发布版本构件的仓库-->
            <id>releases</id>
            <name>Nexus Release Repository</name>
            <url>http://ip:8081/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <!--发布快照版本的仓库-->
            <id>snapshots</id>
            <name>Nexus Snapshot Repository</name>
            <url>http://ip:8081/repository/maven-snapshots/</url>
            <uniqueVersion>true</uniqueVersion>
        </snapshotRepository>
    </distributionManagement>

<!--强制让maven执行检查更新:mvn clean install-U-->
    <repositories>
        <repository>
            <id>data maven repository</id>
            <name>Nexus</name>
            <url>http://ip:8081/repository/maven-public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
<!--<updatePolicy>更新频率：never, always,interval,daily, daily 为默认值-->
                <updatePolicy>always</updatePolicy>
<!--<checksumPolicy>maven检查和检验文件的策略，warn为默认值-->
                <checksumPolicy>warn</checksumPolicy>
            </snapshots>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>data maven plugin repository</id>
            <name>Nexus</name>
            <url>http://ip:8081/repository/maven-public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </pluginRepository>
    </pluginRepositories>
```

出于安全方面的考虑，有时我们要对远程仓库的访问进行认证，一般将认证信息配置在settings.xml中：

```xml
<servers>  
        <server>  
            <id>data maven plugin repository</id>  
            <username>username</username>  
            <password>pwd</password>  
        </server>  
</servers>
```

> 注：这里的id必须与POM中需要认证的repository元素的Id一致。