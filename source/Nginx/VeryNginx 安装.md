## `VeryNginx`安装

`VeryNginx` 是一个功能强大而对人类友好的 `Nginx` 扩展程序

`VeryNginx` 基于 `OpenResty[^openresty]`，所以安装 `VeryNginx` 需要先安装好 `OpenResty`。不过并不用担心安装过程中可能的麻烦，`VeryNginx `自身提供了脚本来进行安装工作

### 安装 `VeryNginx`

#### 下载与安装 `VeryNginx`

```shell
git clone https://github.com/alexazhou/VeryNginx.git
cd VeryNginx
python install.py install verynginx
```

即可一键安装 `VeryNginx `和 以及依赖的 `OpenResty`

#### 更新 `Nginx` 配置文件

有两种方式：

```tex
1.将 VeryNginx 的 nginx.conf 替换 /usr/local/nginx/conf 目录下的文件。
2.使用自己的配置文件，方法如下：
 * 在 http 配置块外部，加入：
	* include /opt/verynginx/verynginx/nginx_conf/in_external.conf;
 * 在 http 配置块内部，加入：
 	* include /opt/verynginx/verynginx/nginx_conf/in_http_block.conf;
 * 在 server 配置块内部，加入：
	* include /opt/verynginx/verynginx/nginx_conf/in_server_block.conf;
```

#### 重启`Nginx`

```shell
[root@localhost nginx]# ./sbin/nginx -s reload
```

#### 登录 `VeryNginx`

访问 `http://ip/verynginx/index.html` 就可以见到 `VeryNginx` 的控制面板。

访问``http://ip/verynginx/index_zh.html`展示中文控制面板

默认用户名和密码是 `verynginx` / `verynginx`








