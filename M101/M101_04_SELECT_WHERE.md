# SQL 문법 - SELECT

Mysql 설치시 제공되는 Sakila, World DB를 활용한 SELECT 예제를 진행한다.


## Select : 데이터 조회하기

기본 형식
``` sql
SELECT [컬럼1, 컬럼2, ...] FROM [테이블]
```

하나의 열을 조회
``` sql
SELECT column1 FROM tablename;
```

하나의 열을 조회
``` sql
SELECT column1 FROM tablename;
```

두 개 이상의 열을 조회
``` sql
SELECT column1, column2 FROM tablename;
```

전체 열을 조회
``` sql
SELECT * FROM tablename;
```



## WHERE문 

테이블에 저장된 전체 데이터를 조회하게되면 시간이 오래 걸려 출력이 늦어지게 되고, 과부하를 줄이기 위해 내가 원하는 조건의 데이터만 WHERE문을 이용하여 조회한다.

### 기본 형식

``` sql
SELECT [컬럼1, 컬럼2, ...] FROM [테이블] WHERE [컬럼] [비교연산자] [값]
```



## 비교연산자 종류 

* 컬럼 = 조건값 : 조건값과 동일한 값을 조회
* 컬럼 !=(<>) 조건값 : 조건값과 같지 않은 값을 조회

* 컬럼 < 조건값 : 조건값 보다 작은 값을 조회
* 컬럼 <= 조건값 : 조건값보다 작거나 같은 값을 조회

* 컬럼 > 조건값 : 조건값보다 큰 값을 조회
* 컬럼 >= 조건값 : 조건값보다 크거나 같은 값을 조회


※ 익숙한 비교 연산자 외 <>, !<, !> 만 숙지하면 된다.  
※ 크기를 비교하는 연산자는 숫자에만 사용하는 것을 권장한다.




## where문에서 비교 연산자 사용


### = 연산자

#### actor 테이블에서 first_name 컬럼의 값이 michael인 데이터 조회

``` sql 
mysql> select * from actor where first_name = 'michael';
+----------+------------+-----------+---------------------+
| actor_id | first_name | last_name | last_update         |
+----------+------------+-----------+---------------------+
|      174 | MICHAEL    | BENING    | 2006-02-15 04:34:33 |
|      185 | MICHAEL    | BOLGER    | 2006-02-15 04:34:33 |
+----------+------------+-----------+---------------------+
2 rows in set (0.00 sec)
```


### <>, != 연산자

#### language 테이블에서 name 컬럼의 값이 English가 아닌 데이터 조회

``` sql
mysql> select * from language where name <> 'English';
+-------------+----------+---------------------+
| language_id | name     | last_update         |
+-------------+----------+---------------------+
|           2 | Italian  | 2006-02-15 05:02:19 |
|           3 | Japanese | 2006-02-15 05:02:19 |
|           4 | Mandarin | 2006-02-15 05:02:19 |
|           5 | French   | 2006-02-15 05:02:19 |
|           6 | German   | 2006-02-15 05:02:19 |
+-------------+----------+---------------------+
5 rows in set (0.00 sec)
```



### > 연산자 
address 테이블에서 city_id 컬럼의 값이 598보다 큰 데이터 조회

``` sql
mysql> select address, district, city_id from address where city_id > 598;
+---------------------+------------+---------+
| address             | district   | city_id |
+---------------------+------------+---------+
| 346 Cam Ranh Avenue | Zhejiang   |     599 |
| 1889 Valparai Way   | Ziguinchor |     600 |
+---------------------+------------+---------+
2 rows in set (0.00 sec)
```


### < 연산자
payment 테이블에서 payment_date 컬럼의 값이 2005-05-25 이전인 데이터 조회

``` sql
mysql> select payment_id, amount, payment_date from payment where payment_date < '2005-05-25';
+------------+--------+---------------------+
| payment_id | amount | payment_date        |
+------------+--------+---------------------+
|       3504 |   2.99 | 2005-05-24 22:53:30 |
|       6003 |   6.99 | 2005-05-24 23:05:21 |
|       6440 |   4.99 | 2005-05-24 23:31:46 |
|       7274 |   1.99 | 2005-05-24 23:11:53 |
|       8987 |   4.99 | 2005-05-24 23:04:41 |
|      11032 |   3.99 | 2005-05-24 23:03:39 |
|      12377 |   2.99 | 2005-05-24 22:54:33 |
|      14728 |   0.99 | 2005-05-24 23:08:07 |
+------------+--------+---------------------+
8 rows in set (0.01 sec)
```


### <= 연산자
payment 테이블에서 payment_id 컬럼이 5보다 작거나 같은 데이터 조회

``` sql
mysql> select payment_id, amount, payment_date from payment where payment_id <= 5;
+------------+--------+---------------------+
| payment_id | amount | payment_date        |
+------------+--------+---------------------+
|          1 |   2.99 | 2005-05-25 11:30:37 |
|          2 |   0.99 | 2005-05-28 10:35:23 |
|          3 |   5.99 | 2005-06-15 00:54:12 |
|          4 |   0.99 | 2005-06-15 18:02:53 |
|          5 |   9.99 | 2005-06-15 21:08:46 |
+------------+--------+---------------------+
5 rows in set (0.00 sec)
```

### >= 연산자

#### payment_id가 5보다 크거나 같은 데이터 조회

``` sql

mysql> select payment_id, amount, payment_date from payment where payment_id >= 5;
+------------+--------+---------------------+
| payment_id | amount | payment_date        |
+------------+--------+---------------------+
|       5    |   9.99 | 2005-06-15 21:08:46 |
|       6    |   3.99 | 2005-06-15 23:04:20 |
|       7    |   2.99 | 2005-06-16 10:32:10 |
|       8    |   4.99 | 2005-06-16 11:01:45 |
|       9    |   1.99 | 2005-06-16 12:40:00 |
+------------+--------+---------------------+
5 rows in set (0.01 sec)

```



## 논리 연산자 종류 및 예시 

### AND 
#### 두 개 이상의 조건이 모두 참일 때 데이터를 조회

레벨이 1이고 성별이 남자인 회원 

``` sql
SELECT * FROM member WHERE mb_level = 1 AND mb_sex = 'M';
```

### OR  
#### 두 개 이상의 조건 중 하나라도 참이면 결과가 참

레벨이 1이거나 성별이 남자인 회원 

``` sql
SELECT * FROM member WHERE mb_level = 1 OR mb_sex = 'M';
```

### NOT 
#### 조건의 반대 값이 참일 때 결과를 반환
레벨이 100이 아닌 회원 
``` sql
SELECT * FROM member WHERE NOT mb_level = 100;
```



### LIKE 
#### 문자열 패턴과 일치하는 데이터를 찾음

이름이 김씨인 회원 (이름에 김을 포함)

``` sql
SELECT * FROM member WHERE mb_name LIKE '김%';
```



### BETWEEN 
#### 지정된 범위 내의 데이터를 조회

회원 포인트가 3000이상 5000이하인 회원

``` sql
SELECT * FROM member WHERE mb_point BETWEEN 3000 and 5000;
```

가입일이 11월1일 ~ 11월 30일인 회원
``` sql
SELECT * FROM member WHERE regDate BETWEEN '2024-11-01' and '2024-11-30';
```


### IN 
#### 피연산자가 값 목록에 하나라도 포함되어 있다면 데이터 조회

레벨이 1, 2, 3인 회원을 조회

``` sql
SELECT * FROM member WHERE mb_level IN (1, 2, 3);
```



### EXISTS 
#### 하위쿼리에 행이 포함되면(결과가 존재하는지) 데이터를 조회

orders 테이블에 주문이 있는 고객을 조회합니다. 하위 쿼리가 결과를 반환하면 EXISTS가 TRUE가 되어 해당 고객을 조회합니다.


``` sql
SELECT * FROM customer
WHERE EXISTS (SELECT 1 FROM orders WHERE orders.customer_id = customer.customer_id);
```






### ALL 

#### 서브쿼리에서 반환된 모든 값과 비교하여 조건을 만족하는 데이터 조회

orders 테이블에서 customer_id가 5인 고객의 모든 주문 금액보다 큰 주문을 조회

``` sql
SELECT * FROM orders
WHERE order_amount > ALL (SELECT order_amount FROM orders WHERE customer_id = 5);
```

### ANY 
#### 서브쿼리에서 반환된 하나 이상의 값과 비교하여 조건을 만족하는 데이터

customer_id가 5인 고객의 주문 금액 중 하나와 같은 금액을 가진 주문을 조회합니다.

``` sql
SELECT * FROM orders
WHERE order_amount = ANY (SELECT order_amount FROM orders WHERE customer_id = 5);
```


### SOME 
#### 서브쿼리에서 반환된 하나 이상의 값과 비교하여 조건을 만족하는 

customer_id가 5인 고객의 주문 금액 중 하나라도 더 작은 주문 금액을 가진 주문을 조회합니다.

```
SELECT * FROM orders
WHERE order_amount > SOME (SELECT order_amount FROM orders WHERE customer_id = 5);

```

※ AND, OR, IN, NOT, BETWEEN, LIKE 정도가 쿼리문 작성시 자주 사용된다.



## NULL

NULL값인 컬럼 조회

``` sql
SELECT * FROM member WHERE mb_addr IS NULL;
```

NULL 값이 아닌 컬럼 조회

``` sql
SELECT * FROM member WHERE mb_addr IS NOT NULL;
```

※ NULL은 특정 값이 아니기때문에 WHERE mb_addr = NULL와 같이 = 연산자를 사용하지 않는다.

















