## Mysql区分大小写(大小写敏感)配置

> Linux下mysql默认区分大小写
>
> Windows下mysql默认不区分大小写

#### 查看是否区分大小写

```sql
mysql> 

+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 0     |
+------------------------+-------+
2 rows in set (0.03 sec)
```

>  lower_case_table_names = 0
> 其中 0：区分大小写，1：不区分大小写 

----

>  MySQL在Linux下数据库名、表名、列名、别名大小写规则是这样的：
> 　　 1、数据库名与表名是严格区分大小写的；
> 　　 2、表的别名是严格区分大小写的；
> 　　 3、列名与列的别名在所有的情况下均是忽略大小写的；
> 　　 4、变量名也是严格区分大小写的； 

#### 修改不区分大小写

* 在my.cnf中的[mysqld]后面添加lower_case_table_names=1
* 重启MYSQL服务

#### 在查看lower_case_table_names =1

```sql
mysql> show variables like 'lower%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 1     |
+------------------------+-------+
2 rows in set (0.03 sec)
```

