## Linux下安装redis

### 一下载

**下载地址：**http://redis.io/download，下载最新稳定版本。

```shell
$ wget http://download.redis.io/releases/redis-5.0.5.tar.gz
$ tar xzf redis-5.0.5.tar.gz
$ cd redis-5.0.5
$ make
```

### 二运行

```shell
$ cd src/
$ src/redis-server
```

### 三验证

```shell
[root@iz2ze0zcgmybh1wlebrk0mz src]# cd /usr/local/redis-5.0.5/src/
[root@iz2ze0zcgmybh1wlebrk0mz src]# ./redis-cli 
127.0.0.1:6379> set test 20
OK
```

