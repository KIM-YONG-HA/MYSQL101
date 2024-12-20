# ORDER BY 문

order by 기본형식은 다음과 같다.

``` sql

SELECT * FROM [테이블명] ORDER BY [컬럼] [ASC|DESC];

```

ASC(ascending) : 오름차순(default 값)

DESC(descening) : 내림차순 



두 개의 열 정렬 예시

``` sql
SELECT * FROM [테이블명] WHERE [조건] ORDER BY [컬럼1], [컬럼2 DESC];
```

1차 정렬 : 컬럼1 ASC

2차 정렬 : 컬럼2 DESC

1차 정렬 후 2차 정렬 진행된다.





# LIMIT 

LIMIT절은 반환할 튜플 수를 지정할 수 있다.
예를 들어 방문자 카운트 테이블 같이 튜플이 굉장히 많은 대형 테이블에서 사용하면 좋다.



## limit 기본 형식 

``` sql
SELECT * FROM [테이블명] LIMIT N;
```
#### 조회결과 : N개 출력


``` sql
mysql> select actor_id, first_name, last_name from actor limit 10;
+----------+------------+--------------+
| actor_id | first_name | last_name    |
+----------+------------+--------------+
|        1 | PENELOPE   | GUINESS      |
|        2 | NICK       | WAHLBERG     |
|        3 | ED         | CHASE        |
|        4 | JENNIFER   | DAVIS        |
|        5 | JOHNNY     | LOLLOBRIGIDA |
|        6 | BETTE      | NICHOLSON    |
|        7 | GRACE      | MOSTEL       |
|        8 | MATTHEW    | JOHANSSON    |
|        9 | JOE        | SWANK        |
|       10 | CHRISTIAN  | GABLE        |
+----------+------------+--------------+
```
#### 조회결과 : 기본 정렬기준으로 10개 출력


## limit 범위
``` sql
SELECT * FROM [테이블명] WHERE [컬럼] = [값] LIMIT N, M;
```
#### N : offset(지정 위치), M : 개수 
#### 조회결과 : N+1 부터 M개 출력

``` sql
mysql> select actor_id, first_name, last_name from actor limit 1, 10;
+----------+------------+--------------+
| actor_id | first_name | last_name    |
+----------+------------+--------------+
|        2 | NICK       | WAHLBERG     |
|        3 | ED         | CHASE        |
|        4 | JENNIFER   | DAVIS        |
|        5 | JOHNNY     | LOLLOBRIGIDA |
|        6 | BETTE      | NICHOLSON    |
|        7 | GRACE      | MOSTEL       |
|        8 | MATTHEW    | JOHANSSON    |
|        9 | JOE        | SWANK        |
|       10 | CHRISTIAN  | GABLE        |
|       11 | ZERO       | CAGE         |
+----------+------------+--------------+
10 rows in set (0.00 sec)

```
#### 조회결과 : 2부터 10개 출력


``` sql
SELECT * FROM [테이블명] LIMIT M OFFSET N;
```
#### 조회결과 : N까지는 건너 뛰고 M개 출력 

``` sql
mysql> select actor_id, first_name, last_name from actor limit 1 offset 10;
+----------+------------+-----------+
| actor_id | first_name | last_name |
+----------+------------+-----------+
|       11 | ZERO       | CAGE      |
+----------+------------+-----------+
1 row in set (0.00 sec)

```
#### 조회결과 : 10까지 건너뛰고 1개 출력





# LIKE 

## LIKE 기본 형식

``` sql

select * from [테이블명] where [열] LIKE [와일드카드 패턴]

```

## 와일드카드

와일드카드 문자는 문자열에서 하나 이상의 문자를 대체할 수 있는데 = 연산자가 아닌 **LIKE 연산자**를 사용하여 패턴을 만들어 데이터를 조회한다.
퍼센트(%)와 언더스코어(_)를 사용하며 퍼센트는 0개 이상의 문자열을 대체, 언더스코어는 단일 문자를 대체한다.


``` sql
(X) select * from [테이블명] where [열] = [패턴]
(O) select * from [테이블명] where [열] LIKE [패턴]
```



### % : 0개 이상의 문자열을 대체 
### _(Under score) : 단일 문자를 대체 


* LIKE 'A%' : A로 시작하는 모든 문자열 
* LIKE '%A' : A로 끝나는 모든 문자열 
* LIKE '%A%' : A가 포함된 모든 문자열 
* LIKE 'A__' : A로 시작하는 두 글자인 문자열 
* LIKE '_A' : A로 끝나는 두 글자인 문자열 
* `LIKE '_A_'` : 세 글자 문자열 중 가운데 글자만 A인 문자열


<!-- 섞어서도 가능 -->



# ESCAPE

퍼센트와 언더스코어는 예약어기 때문에 퍼센트를 포함한 문자열(%%%) 또는 언더스코어를 포함한 문자열(%_%)을 검색할 수 없기 때문데 ESCAPE를 사용하여 조회를 한다.


 
## 퍼센트가 포함된 컬럼 조회
``` sql
SELECT * FROM [테이블명] WHERE [열] LIKE '%\%%' ESCAPE '\';
```

## 언더스코어가 포함된 컬럼 조회
``` sql
SELECT * FROM [테이블명] WHERE [열] LIKE '%\_%' ESCAPE '\';
```



# REGEXP

정규표현식(Regular Expression)을 활용하여 문자열 패턴을 만들어 다양한 방법으로 데이터를 조회할 수 있다.


## REGEXP 기본형식
``` sql
SELECT * FROM [테이블명] WHERE [열] REGEXP [정규식];
```

## 정규표현식 종류

* . : \n을 제외한 임의의 한 문자
* _*_ : 패턴이 0번 이상 반복
* _+_ : 패턴이 1번 이상 반복
* ^ : 문자열의 처음
* $  : 문자열의 끝
* | : or
* [...] : 대괄호 안 문자
* [^...] : 대괄호 안 문자 아닌 문자여
* {n} : n회 반복
* {m,n} : 반복되는 횟수의 최소, 최댓값
 
