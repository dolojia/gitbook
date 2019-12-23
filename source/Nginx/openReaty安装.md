## openReaty安装

* 使用yum安装以下的开发库:

  ```shell
  yum install pcre-devel openssl-devel gcc curl
  ```

  在你的 CentOS 系统中添加 `openresty` 仓库，便于未来安装或更新，（通过 `yum update` 命令）

  ```shell
 sudo yum install yum-utils
   sudo yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo
  ```
  
  然后就可以像下面这样安装软件包，比如 `openresty`：

  ```shell
    sudo yum install openresty
  ```

* 安装完成默认安装路径为

  ```shell
  [root@localhost html]# pwd
  /usr/local/openresty
  ```

* 启动nginx

  ```shell
  [root@localhost html]# cd /usr/local/openresty/nginx/
  [root@localhost nginx]# ./sbin/nginx 
  ```

* 验证是否成功 `curl http://localhost:80/`








