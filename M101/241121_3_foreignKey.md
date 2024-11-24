# 외래키(Foreign Key, FK)의 참조 무결성


## 부모, 자식 테이블 생성 

부모 PK

자식 fk 설정


``` sql

mysql> create table parent_table(col1 int primary key);
Query OK, 0 rows affected (0.02 sec)

mysql> create table child_table(col1 int);
Query OK, 0 rows affected (0.02 sec)

mysql> alter table child_table add foreign key (col1) references parent_table(col1);
Query OK, 0 rows affected (0.08 sec)
Records: 0  Duplicates: 0  Warnings: 0

```


## 부모 PK 데이터 INSERT

``` sql
mysql> insert into parent_table value (10);
Query OK, 1 row affected (0.00 sec)

mysql> insert into parent_table value (20);
Query OK, 1 row affected (0.00 sec)

mysql> insert into parent_table value (30);
Query OK, 1 row affected (0.00 sec)

mysql> insert into parent_table value (40);
Query OK, 1 row affected (0.00 sec)

mysql> insert into parent_table value (50);
Query OK, 1 row affected (0.00 sec)

mysql> select * from parent_table;
+------+
| col1 |
+------+
|   10 |
|   20 |
|   30 |
|   40 |
|   50 |
+------+
5 rows in set (0.00 sec)

```

## 자식 테이블 외래키가 지정된(부모 PK 참조) 필드에 부모 PK가 없는 값 입력
```
mysql> insert into child_table value (100);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`mysite`.`child_table`, CONSTRAINT `child_table_ibfk_1` FOREIGN KEY (`col1`) REFERENCES `parent_table` (`col1`))

```

## 부모테이블에 있는 값 입력 

```
mysql> insert into child_table value (10);
Query OK, 1 row affected (0.00 sec)

mysql> insert into child_table value (20);
Query OK, 1 row affected (0.00 sec)

mysql> insert into child_table value (30);
Query OK, 1 row affected (0.00 sec)

mysql> select * from child_table;
+------+
| col1 |
+------+
|   10 |
|   20 |
|   30 |
+------+
3 rows in set (0.00 sec)
```


## 부모 테이블 pk 삭제 시 오류

자식을 먼저 삭제하고 부모 PK 삭제해야 한다.

```
mysql> delete from parent_table where col1 = 10;
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`mysite`.`child_table`, CONSTRAINT `child_table_ibfk_1` FOREIGN KEY (`col1`) REFERENCES `parent_table` (`col1`))
```


## 자식 삭제 후 부모 데이터 삭제

테이블도 자식, 부모 순으로 삭제해야한다 

``` sql
mysql> delete from child_table where col1 = 10;
Query OK, 1 row affected (0.00 sec)

mysql> delete from parent_table where col1 = 10;
Query OK, 1 row affected (0.04 sec)

mysql> select * from parent_table;
+------+
| col1 |
+------+
|   20 |
|   30 |
|   40 |
|   50 |
+------+
4 rows in set (0.00 sec)

mysql> select * from child_table;
+------+
| col1 |
+------+
|   20 |
|   30 |
+------+
2 rows in set (0.00 sec)
```


## 자식테이블 제약조건 확인 

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