## Docker下vi和vim安装

#### 在docker下，执行apt-get install vi，安装vi，但是返回如下结果

```shell
root@c4c7225c518d:/etc/mysql/mysql.conf.d# apt-get install vim
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package vim
```

#### 这种情况需要先升级apt-get，执行如下命令升级

```shell
root@c4c7225c518d:/# apt-get update
Get:1 http://repo.mysql.com/apt/debian stretch InRelease [21.6 kB]
Get:2 http://security-cdn.debian.org/debian-security stretch/updates InRelease [94.3 kB]               
Get:6 http://security-cdn.debian.org/debian-security stretch/updates/main amd64 Packages [508 kB]                              
Ign:3 http://cdn-fastly.deb.debian.org/debian stretch InRelease                           
Get:4 http://cdn-fastly.deb.debian.org/debian stretch-updates InRelease [91.0 kB]         
Get:7 http://cdn-fastly.deb.debian.org/debian stretch Release [118 kB]                   
Get:8 http://cdn-fastly.deb.debian.org/debian stretch-updates/main amd64 Packages [27.9 kB]
Get:9 http://cdn-fastly.deb.debian.org/debian stretch Release.gpg [2365 B]               
```

#### 更新完成执行install安装vim

```shell
root@c4c7225c518d:/# apt-get install vim
```

