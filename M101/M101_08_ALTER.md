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


``` sql
mysql> desc parent_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | NO   | PRI | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)

mysql> alter table parent_table add col3 int;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | NO   | PRI | NULL    |       |
| col3  | int  | YES  |     | NULL    |       |
+-------+------+------+-----+---------+-------+
2 rows in set (0.00 sec)

```



## 필드 위치 지정 

### AFTER 

AFTER 컬럼 뒤에 위치하며 AFTER를 작성하지 않으면 가장 마지막에 추가된다.


``` sql
ALTER TABLE [테이블명]
ADD [컬럼명] [타입명] [제약조건] AFTER [추가될 필드 앞의 컬럼];
```

``` sql
ALTER TABLE [테이블명]
ADD col2 int [제약조건] AFTER [추가될 필드 앞의 컬럼];
```
``` sql
mysql> alter table parent_table add col2 int after col1;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | NO   | PRI | NULL    |       |
| col2  | int  | YES  |     | NULL    |       |
| col3  | int  | YES  |     | NULL    |       |
+-------+------+------+-----+---------+-------+
3 rows in set (0.00 sec)

```





## FIRST

필드가 제일 앞에 추가된다.

``` sql

mysql> alter table parent_table add col0 int first;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col0  | int  | YES  |     | NULL    |       |
| col1  | int  | NO   | PRI | NULL    |       |
| col2  | int  | YES  |     | NULL    |       |
| col3  | int  | YES  |     | NULL    |       |
+-------+------+------+-----+---------+-------+
4 rows in set (0.00 sec)

```




## 필드 수정


### MODIFY


#### 기본 형식
``` sql
ALTER TABLE PARENT_TABLE MODIFY [컬럼명] [타입];
```


#### col3을 int->varchar(50) 타입으로 변경

``` sql
mysql> alter table parent_table modify col3 varchar(50);
Query OK, 4 rows affected (0.05 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| col0  | int         | YES  |     | NULL    |       |
| col1  | int         | NO   | PRI | NULL    |       |
| col2  | int         | YES  |     | NULL    |       |
| col3  | varchar(50) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```


#### col3을 col0 앞으로 이동

``` sql

mysql> alter table parent_table modify col3 varchar(50) after col0;
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| col0  | int         | YES  |     | NULL    |       |
| col3  | varchar(50) | YES  |     | NULL    |       |
| col1  | int         | NO   | PRI | NULL    |       |
| col2  | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```


#### col3을 제일 앞으로 이동
``` sql
mysql> alter table parent_table modify col3 varchar(50) first;
Query OK, 0 rows affected (0.08 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| col3  | varchar(50) | YES  |     | NULL    |       |
| col0  | int         | YES  |     | NULL    |       |
| col1  | int         | NO   | PRI | NULL    |       |
| col2  | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

#### 컬럼명 타입 기재 없이 작성시 오류

``` sql
mysql> alter table parent_table modify col3 after col2;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'after col2' at line 1
```

#### col3을 col2 뒤로 이동 

``` sql
mysql> alter table parent_table modify col3 varchar(50) after col2;
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| col0  | int         | YES  |     | NULL    |       |
| col1  | int         | NO   | PRI | NULL    |       |
| col2  | int         | YES  |     | NULL    |       |
| col3  | varchar(50) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)

```
**※ 타입을 지정해줘야 오류가 발생하지 않는다.**




## 필드명 변경 

### CHANGE(MySQL 5.x 이상 지원)
필드명 변경시 새로운 컬럼의 타입을 지정해야한다.
``` sql
ALTER TABLE [테이블명] CHANGE [기존컬럼명] [새컬럼명] [새컬럼타입];
```

### CHANGE COLUMN(MySQL 5.x 이상 지원)
CHANGE와 마찬가지로 필드명 변경시 새로운 컬럼의 타입을 지정해야한다.   



``` sql
ALTER TABLE [테이블명] CHANGE COLUMN [기존컬럼명] [새컬럼명] [새컬럼타입];
```
``` sql
mysql> alter table parent_table change col3 column3 varchar(50);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| col0    | int         | YES  |     | NULL    |       |
| col1    | int         | NO   | PRI | NULL    |       |
| col2    | int         | YES  |     | NULL    |       |
| column3 | varchar(50) | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```



### RENAME COLUMN(MySQL 8.0 이상 지원)

타입 지정 없이 필드명만 변경 

``` sql
ALTER TABLE [테이블명] RENAME COLUMN [기존컬럼명] TO [새컬럼명];
```
``` sql
mysql> alter table parent_table rename column col2 to column2;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| col0    | int         | YES  |     | NULL    |       |
| col1    | int         | NO   | PRI | NULL    |       |
| column2 | int         | YES  |     | NULL    |       |
| column3 | varchar(50) | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```


## 필드 삭제 


### DROP

한 개의 필드 삭제

``` SQL
ALTER TABLE [테이블명] DROP [기존컬럼명];
```
``` sql
mysql> alter table parent_table drop column3;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+---------+------+------+-----+---------+-------+
| Field   | Type | Null | Key | Default | Extra |
+---------+------+------+-----+---------+-------+
| col0    | int  | YES  |     | NULL    |       |
| col1    | int  | NO   | PRI | NULL    |       |
| column2 | int  | YES  |     | NULL    |       |
+---------+------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

여러 개의 필드 삭제

``` SQL
mysql> alter table parent_table drop col0, drop column2;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc parent_table;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| col1  | int  | NO   | PRI | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)
```


PK, FK 삭제시 오류
``` sql
mysql> alter table parent_table drop col1;
ERROR 1090 (42000): You can't delete all columns with ALTER TABLE; use DROP TABLE instead
mysql> alter table child_table drop col1;
ERROR 1090 (42000): You can't delete all columns with ALTER TABLE; use DROP TABLE instead
mysql> desc child_table;
```

아래의 링크에서 확인 


[[MYSQL][06] 외래키(Foreign Key, FK)의 참조 무결성](https://fronttoback.tistory.com/54)

