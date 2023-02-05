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
```sql

```

3. FULL OUTER JOIN 

```sql

```

4. CROSS JOIN
```sql

```

 
5. SELF JOIN 
같은 테이블 명을 쓰고 별칭만 A,B와 같이 다르게 한다. 

```sql
SELECT A
FROM 
  ON
WHERE
```
