#  gitlab邮箱设置

### 配置文件

```shell
[root@localhost gitlab]# vim /etc/gitlab/gitlab.rb
```

### 修改内容

```yaml
gitlab_rails['gitlab_email_enabled'] = true
gitlab_rails['gitlab_email_from'] = 'alert@xxx.cn'
gitlab_rails['gitlab_email_display_name'] = 'gitlab-admin'
gitlab_rails['gitlab_email_reply_to'] = 'alert@xxx.cn'
#不加入验证
gitlab_rails['smtp_openssl_verify_mode'] = 'none'

gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.xxx.cn"
gitlab_rails['smtp_port'] = 25
gitlab_rails['smtp_user_name'] = "alert@xxx.cn"
gitlab_rails['smtp_password'] = "password"
gitlab_rails['smtp_domain'] = "xxx.cn"
gitlab_rails['smtp_authentication'] = "login"
```

### 重启gitlab

```shell
[root@localhost gitlab]# sudo gitlab-ctl reconfigure
```

### 验证邮件发送

```shell
[root@localhost gitlab]# gitlab-rails console
Loading production environment (Rails 4.2.8)
2.3.0 :001 > Notify.test_email('xxx@xxx.com', 'Message Subject', 'Message Body').deliver_now
```

