# ALTER

## 예제 쿼리문

### 부모 테이블 생성

``` sql
mysql> create table parent_table(col1 int);
Query OK, 0 rows affected (0.02 sec)
mysql> desc parent_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | YES  |     | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)
```


### 자식 테이블 생성

``` sql 
mysql> create table child_table(col1 int);
Query OK, 0 rows affected (0.02 sec)

mysql> desc child_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | YES  |     | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)

```

## PK의 추가 및 삭제 

### PK 추가 

``` SQL
ALTER TABLE [테이블명] ADD PRIMARY KEY(PK_COLUMN);
```
``` sql
mysql> alter table parent_table add primary key(col1);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | NO   | PRI | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)

```



### PK 삭제
``` sql
ALTER TABLE [테이블명] DROP PRIMARY KEY;
```

``` sql
mysql> alter table parent_table drop primary key;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | NO   |     | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)
```



## FK의 추가 및 삭제


### FK 추가 

외래키 추가시 자식 테이블에서 **REFERENCES** 사용하여 부모 테이블의 PK를 참조한다.

``` SQL
ALTER TABLE CHILD_TABLE ADD FOREIGN KEY(C_FK_COLUMN) 
REFERENCES PARENT_TABLE(P_PK_COLUMN)
```

``` sql
mysql> alter table child_table add foreign key(col1) references parent_table(col1);
Query OK, 0 rows affected (0.06 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc child_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | YES  | MUL | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)

```


## FK 삭제

CONSTRAINT 제약 조건을 찾아서 DROP 한다.



``` sql
mysql> show create table child_table;
+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table       | Create Table                                                                                                                                                                                                                                |
+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| child_table | CREATE TABLE `child_table` (
  `col1` int DEFAULT NULL,
  KEY `col1` (`col1`),
  CONSTRAINT `child_table_ibfk_1` FOREIGN KEY (`col1`) REFERENCES `parent_table` (`col1`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

``` sql

mysql> alter table child_table drop constraint child_table_ibfk_1;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc child_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | YES  | MUL | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)

```




## 필드 추가 

### ADD 기본형식

``` sql
ALTER TABLE [테이블명]
ADD [컬럼명] [타입명] [제약조건];
```

### ADD 여러 필드 입력
``` sql
ALTER TABLE [테이블명]
ADD [컬럼명] [타입명] [제약조건],
ADD [컬럼명] [타입명] [제약조건],
ADD [컬럼명] [타입명] [제약조건];
```






## 필드 위치 지정 

### AFTER 

``` sql
ALTER TABLE [테이블명]
ADD [컬럼명] [타입명] [제약조건] AFTER [추가될 필드 앞의 컬럼];
```

``` sql
ALTER TABLE [테이블명]
ADD col2 int [제약조건] AFTER [추가될 필드 앞의 컬럼];
```



## FIRST
```
ALTER TABLE PARENT_TABLE MODIFY COL0 int FIRST;
```





## 필드 수정


### MODIFY

``` sql
ALTER TABLE PARENT_TABLE MODIFY COL3 int AFTER COL2;
```






## 필드 삭제 


### DROP

``` SQL
```

``` SQL
```



## 필드명 변경 

### CHANGE
```
ALTER TABLE PARENT_TABLE CHANGE COL1 COLUMN1 int;
```