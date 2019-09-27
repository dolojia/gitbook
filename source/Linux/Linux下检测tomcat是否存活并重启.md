# Linux下检测tomcat是否存活并重启

#### 1. 编写tomcat检测存活重启脚本

* 只检测进程是否存在脚本

  ````shell
  #!/bin/sh  
  # function:自动监控tomcat进程，挂了就执行重启操作 
  
  #tomcat文件名称
  TOMCAT_NAME=xxxxx
  #tomcat部署路径
  TOMCAT_HOME=/home/local/tomcat
  # 日志输出  
  TOMCAT_MONITOR_LOG=/usr/local/shell/tomcatMonitor.log
  
  # 获取tomcat PPID  
  TOMCAT_ID=$(ps -ef |grep tomcat |grep -w $TOMCAT_NAME|grep -v 'grep'|awk '{print $2}')
  # tomcat_startup  
  START_TOMCAT=$TOMCAT_HOME/$TOMCAT_NAME/bin/startup.sh
  
  Monitor()  
  {  
    echo "[info]开始监控tomcat...[$(date +'%F %H:%M:%S')]"  
    if [ $TOMCAT_ID ];then  
      echo "[info]$TOMCAT_NAME进程ID为:$TOMCAT_ID."  
      echo "$TOMCAT_NAME 状态正常... ending."  
    else  
      echo "[error]进程不存在!$TOMCAT_NAME自动重启..."  
      echo "[info]$START_TOMCAT,请稍候......"  
      $START_TOMCAT  
    fi  
    echo "------------------------------"  
  }  
  Monitor>>$TOMCAT_MONITOR_LOG
  ````

* 带url状态检测脚本

  ````shell
  #!/bin/sh  
  # function:自动监控tomcat进程，挂了就执行重启操作  
  
  #tomcat文件名称
  TOMCAT_NAME=xxxxx
  #tomcat部署路径
  TOMCAT_HOME=/home/local/tomcat
  #定义要监控的页面地址（写比较简单页面即可）
  WEB_URL=http://localhost:8080/xxxx/
  # 日志输出  
  TOMCAT_MONITOR_LOG=/home/local/shell/tomcatMonitor.log
  
  # tomcat_startup  
  START_TOMCAT=$TOMCAT_HOME/$TOMCAT_NAME/bin/startup.sh
  # 获取tomcat PPID  
  TOMCAT_ID=$(ps -ef |grep tomcat |grep -w $TOMCAT_NAME|grep -v 'grep'|awk '{print $2}')
  
  # curl 日志-o /dev/null 屏蔽原有输出信息
  CURL_MONITOR_LOG=/dev/null
  
  Monitor()  
  {
    echo "[info]开始监控$TOMCAT_NAME...[$(date +'%F %H:%M:%S')]"  
    #判断Tomcat进程是否存在
    if [ $TOMCAT_ID ];then
      echo "[info]$TOMCAT_NAME进程ID为:$TOMCAT_ID.继续检测......"  
      #检测是否启动成功（成功的话页面会返回状态"200"）
      TomcatServiceCode=$(curl -I -m 10 -o $CURL_MONITOR_LOG -s -w %{http_code} $WEB_URL)
      if [ $TomcatServiceCode -eq 200 ];then
          echo "[info] curl $WEB_URL返回码为$TomcatServiceCode."
          echo "$TOMCAT_NAME 状态正常... ending."  
      else
        echo "[error]$TOMCAT_NAME出错，请注意..状态码为$TomcatServiceCode，开始重启$TOMCAT_NAME..."
        #杀掉原tomcat进程
        kill -9 $TOMCAT_ID
        sleep 3
        $START_TOMCAT
      fi
    else
      echo "[error]进程不存在!$TOMCAT_NAME自动重启..."  
      echo "[info]$START_TOMCAT,请稍候......"  
      $START_TOMCAT
    fi
    echo "------------------------------"  
  }
  Monitor>>$TOMCAT_MONITOR_LOG
  ````

#### 2. 对该脚本赋予执行权限

* 赋执行权限

  `````shell
  [root@IDCWX05 shell]# chmod -R 777 tomcat-monitor.sh 
  `````


#### 3. 修改定时任务启动器

* 查看定时任务列表

  ````shell
  [root@IDCWX05 shell]# crontab -l
  05 09 * * * /usr/local/shell/time/build-dos2unix.sh
  */5 * * * * /usr/local/shell/tomcat-monitor.sh
  ````

* 编辑定时任务启动器

  ````shell
  [root@IDCWX05 shell]# crontab -e
  ````

* 在打开的脚本页输入（每5分钟执行一次）

  ````shell
  */5 * * * * /usr/local/shell/tomcat-monitor.sh  
  ````

* 正常保存退出即可生效

* 查看crontab状态 

  ````shell
  [root@IDCWX05 shell]# tail -f /var/log/cron
  Sep 10 20:19:57 IDCWX05 crontab[21591]: (root) LIST (root)
  Sep 10 20:20:01 IDCWX05 CROND[21599]: (root) CMD (/usr/local/shell/check_tomcat_live.sh)
  Sep 10 20:20:01 IDCWX05 CROND[21600]: (root) CMD (/usr/lib64/sa/sa1 1 1)
  Sep 10 20:21:01 IDCWX05 crontab[21616]: (root) BEGIN EDIT (root)
  Sep 10 20:21:02 IDCWX05 crontab[21616]: (root) END EDIT (root)
  Sep 10 20:25:01 IDCWX05 CROND[21627]: (root) CMD (/usr/local/shell/check_tomcat_live.sh)
  Sep 10 20:30:01 IDCWX05 CROND[21692]: (root) CMD (/usr/lib64/sa/sa1 1 1)
  Sep 10 20:30:01 IDCWX05 CROND[21693]: (root) CMD (/usr/local/shell/check_tomcat_live.sh)
  Sep 10 20:35:02 IDCWX05 CROND[21755]: (root) CMD (/usr/local/shell/check_tomcat_live.sh)
  Sep 10 20:35:32 IDCWX05 crontab[21766]: (root) LIST (root)
  ````

  ​		

