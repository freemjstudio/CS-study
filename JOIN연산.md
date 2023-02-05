# JOIN 연산

![image](https://user-images.githubusercontent.com/41604678/216812855-919bbb20-9100-4881-93cf-f11c1843085b.png)


[도서]  
|책번호|책명|
|----|---|
|111|운영체제|
|222|자료구조|
|555|컴퓨터구조|

[도서가격]  
|책번호|가격|
|----|---|
|111|20000|
|222|25000|
|333|10000|
|444|15000|

1. INNER JOIN 
```sql 
SELECT A.책번호, A.책명, B.가격 
FROM 도서 A JOIN 도서가격 B
ON A.책번호 = B.책번호 
```
|A.책번호|A.책명|B.가격|
|------|-----|-----|
|111|운영체제|20000|
|222|자료구조|25000|


2. OUTER JOIN 
1) LEFT OUTER JOIN 
![image](https://user-images.githubusercontent.com/41604678/216813380-7fabf35b-c821-4124-adc4-4178f410a87a.png)

```sql
SELECT A.책번호, A.책명, B.책번호, B.가격 
FROM 도서 A LEFT JOIN 도서가격 B
ON A.책번호 = B.책번호
```

|A.책번호|A.책명|B.책번호|B.가격|
|------|-----|-----|------|
|111|운영체제|111|20000|
|222|자료구조|222|25000|
|555|컴퓨터구조|NULL|NULL|

2) RIGHT OUTER JOIN 
![image](https://user-images.githubusercontent.com/41604678/216813397-9506774f-5e73-49d0-83bd-a864c5d8d455.png)

3. FULL OUTER JOIN 

```sql

```

4. CROSS JOIN
각각의 테이블의 곱집합이다. 

```sql

```

 
5. SELF JOIN 
같은 테이블 명을 쓰고 별칭만 A,B와 같이 다르게 한다. 

```sql
SELECT 책번호
FROM 도서 
  ON
WHERE
UNION
```
# SQL 집합 연산자 
테이블을 집합 개념으로 보고 두 테이블 연산 (두 쿼리문 사이에)에 집합 연산자를 사용할 수 있다
UNION, UNION ALL, INTERSECT, MINUS 

* UNION : 중복된 행은 제거한 후 합집합 결과 반환  
* UNION ALL : 중복된 행을 제거하지 않고 합집합 결과 반환  
* INTERSECT : 두 쿼리 결과에 공통적으로 존재하는 결과 반환 
* MINUS : 첫 쿼리에 있고 두번째 쿼리에는 없는 결과만 반환 

다음과 같은 형식으로 사용한다. 

```sql
SELECT 책번호
FROM 도서 
WHERE 책번호 = 111 OR 책번호 = 222
UNION
SELECT 책번호 
FROM 도서가격
WHERE 가격 >= 15000
```sql

|책번호|
|----|
|111|
|222|
|444|
