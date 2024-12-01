``` sql

load data local infile 'C:\\판매.txt' into table sales.sale 
fields terminated by '\t' 
lines terminated by '\r\n';

```

```
ERROR 3948 (42000): Loading local data is disabled; this must be enabled on both the client and server sides
```


``` sql
SHOW VARIABLES LIKE 'local_inflie';
set global local_infile=true;
```


```
ERROR 2068 (HY000): LOAD DATA LOCAL INFILE file request rejected due to restrictions on access.
```


```
mysql --local-infile=1 -u root -p
```



```
show table status where name = '테이블명' 
 alter table table_name auto_increment=1; 

```