## Oracle常用语句

#### 修改表字段长度

```sql
alter table table_name modify  column_name1 varchar2(15),column_name2 varchar2(30);
```



#### 创建序列

```sql
CREATE SEQUENCE  SEQUENCE  _name MINVALUE 1 MAXVALUE 9999999999 INCREMENT BY 1 START WITH 1 CACHE 20 NOORDER  CYCLE  NOKEEP  NOSCALE  GLOBAL ;
```



#### 新加字段

```sql
alter table  表名  add 字段名  NUMBER(16,2)  default 0 ;
comment on column .refund_amount is '字段描述';
```

