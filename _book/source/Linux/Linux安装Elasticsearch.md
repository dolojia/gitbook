### Linux安装Elasticsearch

####　下载安装包

* 官网下载地址`https://www.elastic.co/cn/downloads/elasticsearch`

* 解压安装包

  ```shell
  tar -zxvf elasticsearch-7.3.1-linux-x86_64.tar.gz
  ```

* ##### 修改 config 目录下的 elasticsearch.yml 文件

  ```shell
  // 去掉行开头的 # 并重命名集群名，这里命名为 compass
  cluster.name: compass
  // 去掉行开头的 # 并重命名节点名，这里命名为 node-1
  node.name: node-1
  ```

* ##### 启动ES： 版本> = 5.0.0 时，是不能用超级管理员运行的，此时需要切换到普通账号或者新建 ES 账号

  ```shell
  [root@localhost elasticsearch-7.3.1]# sh bin/elasticsearch
  OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
  [2019-08-29T10:11:09,644][WARN ][o.e.b.ElasticsearchUncaughtExceptionHandler] [localhost.dolojia] uncaught exception in thread [main]
  org.elasticsearch.bootstrap.StartupException: java.lang.RuntimeException: can not run elasticsearch as root
  	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:163) ~[elasticsearch-7.3.1.jar:7.3.1]
  	at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:150) ~[elasticsearch-7.3.1.jar:7.3.1]
  	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86) ~[elasticsearch-7.3.1.jar:7.3.1]
  	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:124) ~[elasticsearch-cli-7.3.1.jar:7.3.1]
  	at org.elasticsearch.cli.Command.main(Command.java:90) ~[elasticsearch-cli-7.3.1.jar:7.3.1]
  	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:115) ~[elasticsearch-7.3.1.jar:7.3.1]
  	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:92) ~[elasticsearch-7.3.1.jar:7.3.1]
  Caused by: java.lang.RuntimeException: can not run elasticsearch as root
  	at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:105) ~[elasticsearch-7.3.1.jar:7.3.1]
  	at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:172) ~[elasticsearch-7.3.1.jar:7.3.1]
  	at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:349) ~[elasticsearch-7.3.1.jar:7.3.1]
  	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:159) ~[elasticsearch-7.3.1.jar:7.3.1]
  	... 6 more
  ```

* ##### 创建es专属用户启动

  ① 新建用户组 elasticsearch

  ```
  $ groupadd elasticsearch
  ```

  ② 新建用户并指定用户组

  ```
  $ useradd -g elasticsearch elasticsearch
  ```

  ③ 修改 ES 目录所属者

  ```
  $ chown -R elasticsearch:elasticsearch elasticsearch
  ```

  ④ 切换用户后再次启动

  ```
  $ su elasticsearch
  ```

* ##### 只能使用127.0.01或者localhost访问，使用ip地址无法访问

  ```shell
  #修改 elasticsearch.yml 中的「network.host」
  network.host: 0.0.0.0
  ```

* 验证是否启动成功

  ```shell
  [root@localhost opt]# curl http://localhost:9200
  {
    "name" : "localhost.dolojia",
    "cluster_name" : "elasticsearch",
    "cluster_uuid" : "wJIrmAaDRu--MfaRHtr70w",
    "version" : {
      "number" : "7.3.1",
      "build_flavor" : "default",
      "build_type" : "tar",
      "build_hash" : "4749ba6",
      "build_date" : "2019-08-19T20:19:25.651794Z",
      "build_snapshot" : false,
      "lucene_version" : "8.1.0",
      "minimum_wire_compatibility_version" : "6.8.0",
      "minimum_index_compatibility_version" : "6.0.0-beta1"
    },
    "tagline" : "You Know, for Search"
  }
  ```

  

