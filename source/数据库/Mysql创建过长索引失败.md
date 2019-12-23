## Mysql创建过长索引失败

> 在MySQL 5.6版本的数据库中修改InnoDB表字段长度时遇到了"ERROR 1071 (42000): Specified key was too long; max key length is 767 bytes"错误。最终按网上资料如下方式解决，特此记录

```sql
mysql> use MyDB;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
 
Database changed
mysql> CREATE TABLE `TEST` (
    ->   `CODE_NAME` varchar(100) NOT NULL DEFAULT '',
    ->   `CODE_SEQ` smallint(6) NOT NULL DEFAULT '1',
    ->   `ACTIVE` char(1) DEFAULT 'Y',
    ->   `CODE_VALUE1` varchar(250) DEFAULT NULL,
    ->   PRIMARY KEY (`CODE_NAME`,`CODE_SEQ`),
    ->   KEY `IDX_GEN_CODE` (`CODE_NAME`,`CODE_VALUE1`)
    -> ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.02 sec)
 
 
mysql> ALTER TABLE TEST MODIFY CODE_VALUE1 VARCHAR(350);
ERROR 1071 (42000): Specified key was too long; max key length is 767 bytes
mysql> 
```

### 解决方案：

> **1：启用系统变量**innodb_large_prefix
>
> 注意：光有这个系统变量开启是不够的。必须满足下面几个条件：
>
> ​    1： 系统变量innodb_large_prefix为ON
>
> ​    2： 系统变量innodb_file_format为Barracuda
>
> ​    3： ROW_FORMAT为DYNAMIC或COMPRESSED
>
> 如下测试所示：

```sq
mysql> show variables like '%innodb_large_prefix%';
+---------------------+-------+
| Variable_name       | Value |
+---------------------+-------+
| innodb_large_prefix | OFF   |
+---------------------+-------+
1 row in set (0.00 sec)
 
mysql> set global innodb_large_prefix=on;
Query OK, 0 rows affected (0.00 sec)
 
mysql> ALTER TABLE TEST MODIFY CODE_VALUE1 VARCHAR(350);
ERROR 1709 (HY000): Index column size too large. The maximum column size is 767 bytes.
mysql> 
mysql> show variables like '%innodb_file_format%';
+--------------------------+-----------+
| Variable_name            | Value     |
+--------------------------+-----------+
| innodb_file_format       | Antelope  |
| innodb_file_format_check | ON        |
| innodb_file_format_max   | Barracuda |
+--------------------------+-----------+
3 rows in set (0.01 sec)
 
mysql> set global innodb_file_format=Barracuda;
Query OK, 0 rows affected (0.00 sec)
 
mysql> ALTER TABLE TEST MODIFY CODE_VALUE1 VARCHAR(350);
ERROR 1709 (HY000): Index column size too large. The maximum column size is 767 bytes.
mysql> 
 
mysql> 
mysql> show table status from MyDB where name='TEST';
*************************** 1. row ***************************
           Name: TEST
         Engine: InnoDB
        Version: 10
     Row_format: Compact
           Rows: 0
 Avg_row_length: 0
    Data_length: 16384
Max_data_length: 0
   Index_length: 16384
      Data_free: 0
 Auto_increment: NULL
    Create_time: 2018-09-20 13:53:49
    Update_time: NULL
     Check_time: NULL
      Collation: utf8_general_ci
       Checksum: NULL
 Create_options: 
        Comment: 
 
mysql>  ALTER TABLE TEST ROW_FORMAT=DYNAMIC;
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0
 
mysql> show table status from MyDB where name='TEST';
*************************** 1. row ***************************
           Name: TEST
         Engine: InnoDB
        Version: 10
     Row_format: Dynamic
           Rows: 0
 Avg_row_length: 0
    Data_length: 16384
Max_data_length: 0
   Index_length: 16384
      Data_free: 0
 Auto_increment: NULL
    Create_time: 2018-09-20 14:04:05
    Update_time: NULL
     Check_time: NULL
      Collation: utf8_general_ci
       Checksum: NULL
 Create_options: row_format=DYNAMIC
        Comment: 
1 row in set (0.00 sec)
 
ERROR: 
No query specified
 
mysql> ALTER TABLE TEST MODIFY CODE_VALUE1 VARCHAR(350);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

