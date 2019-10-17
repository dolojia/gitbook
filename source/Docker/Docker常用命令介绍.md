## Docker常用命令介绍

### 一、帮助命令

* `docker version`
* `docker info`
* `docker --help`
  * `docker run --help` 查看`run`命令示例

### 二、镜像命令

- `docker images` 查看镜像

  - -a 列出所有镜像
  - -q 只显示镜像id
  - --digests 显示摘要信息
  - --no-trunc

- `docker search` 搜索镜像

  ```shell
  #docker search 镜像名
  [root@localhost docker]# docker search tomcat 
  ```

- `docker pull` 下载镜像

  ```shell
  #docker pull 镜像名
  [root@localhost docker]# docker pull tomcat 
  ```

- `docker rmi` 删除镜像

  - 删除单个`docker rmi -f 镜像名`
  - 删除多个`docker rmi -f 镜像名1:tag1 镜像名2:tag2`
  - 删除全部`docker rmi -f ${docker images -q}`
  
- `docker commit `将现有容器制作为镜像

  ```shell
  #docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[标签]
  [root@localhost docker]# docker commit -m="描述信息" -a="作者" b4vfd526 tocmat-test2:2.0
  ```

### 三、容器命令

* `docker run` 新建并启动容器

  ```shell
  #docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
  [root@localhost docker]# docker run -d -p 8080:8080 --name tomcat_server tomcat
  ```

  * -- name 为容器取一个新名字

  * -d 后台运行

  * -i 交互方式运行

  * -t 启动一个伪终端

  * -p 指定端口映射

  * -q 随机端口映射

  * --env 配置环境变量

    ```shell
    [root@localhost docker]# docker run --env config_env=dev -d -p 8080:8080 --name tomcat_server tomcat
    ```

* `docker ps` 查看容器

  ```shell
  [root@localhost docker]# docker ps -a
  ```

  * -a 列出所有容器
  * -l 列出最近创建的容器
  * -n 显示最近穿件的N个容器
  * -q 只显示容器id

* 退出容器

  * `exit` 退出并停止容器
  * `ctrl+p+q` 退出不停止容器

* `docker start`启动容器

  ```shell
  docker start 容器id/名称
  ```

* `docker restart`重启容器

  ```shell
  docker restart 容器id/名称
  ```

* `docker stop`停止容器

  ```shell
  docker stop 容器id/名称
  ```

* `docker kill`强制停止容器

  * docker kill 容器id/名称

* `docker -rm `删除容器

  * `docker -rm` 容器id
  * `docker rm $(docker ps -aq)`删除没在运行的容器

* `docker logs` 查看容器日志

  ```shell
  #docker logs [OPTIONS] CONTAINER
  [root@localhost docker]# docker logs -t -f --tail=10 6d1fd54ca10ad8f9e99
  ```

  也可以在主机目录`/var/lib/docker/containers/`下找到对应容器日志

* `docker top` 查看容器内进程

  ```shell
  [root@localhost docker]# docker top 6d1fd54ca10ad8f9e
  ```

* `docker inspect` 查看容器详细信息

  ```shell
  [root@localhost docker]# docker inspect 6d1fd54ca10ad8
  ```

* `docker exec` 进入正在运行的容器

  ```shell
  #docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
  [root@localhost docker]# docker exec -it 6d1fd54ca10ad8f9 /bin/bash
  root@6d1fd54ca10a:/usr/local/tomcat# 
  ```

* `docker cp` 从容器内部拷贝文件至主机

  ```shell
  #docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
  #docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
  [root@localhost docker]# docker cp 6244e9d112a0:BUILDING.txt /home/
  ```

  

