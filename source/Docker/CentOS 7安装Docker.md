# CentOS 7安装Docker

Docker支持以下的CentOS版本：

- CentOS 7 (64-bit)
- CentOS 6.5 (64-bit) 或更高的版本

查看系统版本

```shell
[root@localhost ~]# uname -r
3.10.0-957.el7.x86_64
```

Docker 运行在 CentOS 7 上，要求系统为64位、系统内核版本为 3.10 以上。

## 使用 yum 安装（CentOS 7下）

### 安装 Docker

从 2017 年 3 月开始 docker 在原来的基础上分为两个分支版本: Docker CE 和 Docker EE。

Docker CE 即社区免费版，Docker EE 即企业版，强调安全，但需付费使用

本文使用 Docker CE 的安装

#### 1.移除旧的版本：

```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

#### 2.安装一些必要的系统工具：

```shell
[root@localhost ~]# sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

#### 3.添加软件源信息

```shell
[root@localhost ~]# sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

#### 4.更新 yum 缓存

```shell
[root@localhost ~]# sudo yum makecache fast
```

#### 5.安装 Docker-ce

```shell
[root@localhost ~]# sudo yum -y install docker-ce
```

#### 6.启动 Docker 后台服务

```shell
[root@localhost ~]# systemctl start docker.service
#添加开机启动服务
[root@localhost ~]# systemctl enable docker.service
```

#### 7.容器随docker启动而启动

> --restart=always
>
> ```
> Flag	Description
> no		不自动重启容器. (默认value)
> on-failure 	容器发生error而退出(容器退出状态不为0)重启容器
> unless-stopped 	在容器已经stop掉或Docker stoped/restarted的时候才重启容器
> always 	在容器已经stop掉或Docker stoped/restarted的时候才重启容器
> ```

##### 如果已经过运行的项目

```
#如果已经启动的项目，则使用update更新：
docker update --restart=always CONTAINER_NAME
```

#### 8.测试运行 hello-world

```shell
[root@localhost ~]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete 
Digest: sha256:c3b4ada4687bbaa170745b3e4dd8ac3f194ca95b2d0518b417fb47e5879d9b5f
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

由于本地没有hello-world这个镜像，所以会下载一个hello-world的镜像，并在容器内运行。

到此，Docker 在 CentOS 系统的安装完成。



---------

## 镜像加速

鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，我们可以需要配置加速器来解决

新版的 Docker 使用 /etc/docker/daemon.json（Linux） 或者 %programdata%\docker\config\daemon.json（Windows） 来配置 Daemon。

请在该配置文件中加入（没有该文件的话，请先建一个）：

```shell
{
  "registry-mirrors": ["加速器地址"]
}
```

修改之后重启docker

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

获取阿里加速器地址

> 登录阿里云 https://cr.console.aliyun.com/  
>
> 通过搜索镜像加速器找到功能入口
>
> 根据提示获取地址

## 删除 Docker CE

```shell
$ sudo yum remove docker-ce
$ sudo rm -rf /var/lib/docker
```

