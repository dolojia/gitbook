### contos7防火墙关闭

#### 查看防火前状态

```shell
[es@localhost bin]$ systemctl status firewalld.service
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since 二 2019-09-03 14:25:37 CST; 20min ago
     Docs: man:firewalld(1)
 Main PID: 6585 (firewalld)
   CGroup: /system.slice/firewalld.service
           └─6585 /usr/bin/python -Es /usr/sbin/firewalld --nofork --nopid
```

#### 关闭防火墙`需输入root密码确认权限`

```shell
[es@localhost bin]$ systemctl stop firewalld.service
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to manage system services or units.
Authenticating as: root
Password: 
==== AUTHENTICATION COMPLETE ===
```

再次查看状态：`Active: active (running)`变为`Active: inactive (dead)`说明关闭成功

#### 永久关闭防火墙

```shell
[es@localhost bin]$ systemctl disable firewalld.service
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-unit-files ===
Authentication is required to manage system service or unit files.
Authenticating as: root
Password: 
==== AUTHENTICATION COMPLETE ===
```