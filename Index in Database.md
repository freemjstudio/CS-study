# Index in Database

```sql
SELECT *
FROM customer
WHERE first_name = 'Minsoo'
```

| id | last_name | first_name | address |
| --- | --- | --- | --- |
| .. | .. | .. | .. |
| .. | .. | … | .. |

100만개의 Tuple

만약에 index가 걸려있지 않다면, full scan을 하게 되고 O(N)의 시간복잡도를 가진다. 하지만 first_name에 index가 걸려있다면, full-scan보다 더 빨리 찾을 수 있으며, B-tree 기반일 때 O(logN)의 시간복잡도를 가진다.

## Index를 쓰는 이유

- 조건을 만족하는 튜플들을 빠르게 조회하기 위해
- 빠르게 정렬(order by)하거나 그룹핑(group by) 하기 위해!

```sql
SELECT * FROM CUSTOMER WHERE FIRST_NAME == 'MINSOO'
DELETE FROM LOGS WHERE LOG_DATATIME < '2022-01-01'
UPDATE EMPLOYEE SET SALARY = SALARY * 1.5 WHERE DEPT_ID = 1001
SELECT * FROM EMPLOYEE E JOIN DEPARTMENT D ON E.DEPT_ID = D.ID
```

### 2개의 쿼리가 사용될 때

| id | name | team_id | backnumber |
| --- | --- | --- | --- |
| … | … | … | … |

```sql
SELECT * FROM player WHERE name = 'MESSI'
SELECT * FROM player WHERE team_id = 105 and backnumber = 7
```

- 이름은 중복이 생길 수 있기 때문에, 중복을 허용할 수 있는 인덱스
    
    ```sql
    CREATE INDEX player_name_idx ON player(name);
    ```
    
    player의 name이라는 어트리뷰트에 index를 적용시킨다. index를 적용시키게 되면, 1번째 SELECT문은 이제 index를 사용해서 가져오게 된다.
    
- 팀 내에는 등번호가 유일하기 때문에, team_id and backnumber를 사용한다. 2개의 어트리뷰트를 합쳐서 인덱스로 만들어줄 수 있다.
    
    ```sql
    CREATE UNIQUE INDEX team_id_backnumber_idx ON player(team_id, backnumber)
    ```
    

### 생성시에 index

```sql
CREATE TABLE palyer (
	id INT PRIMARY KEY,
	name VARCHAR(20) NOT NULL,
  team_id INT,
  backnumber INT,
	INDEX player_name_idx (name),
	UNIQUE INDEX team_id_backnumber_idx (team_id, backnumber) // Composite INDEX 
);
```

대부분 primary key에는 index가 자동 생성된다. (in RDBMS)

- 현재 테이블에 어떤 인덱스가 걸려있는지 궁금할 때
    
    ```sql
    SHOW INDEX FROM player;
    ```
    

## B-tree 기반 index가 동작하는 방식

B-tree based INDEX(a)

| a | ptr |
| --- | --- |
| 1 |  |
| 2 |  |
| 2 |  |
| 3 |  |
| 5 |  |
| 7 |  |
| 7 |  |
| 7 |  |
| 9 |  |
| 13 |  |

정렬이 된 상태에서 저장된다.

prt 형태로 저장된다.

MEMBERS

| a | b | c |
| --- | --- | --- |
| 3 |  |  |
| 7 | 78 |  |
| 1 |  |  |
| 2 |  |  |
| 9 |  |  |
| 7 | 95 |  |
| 13 |  |  |
| 5 |  |  |
| 7 | 80 |  |
| 2 |  |  |

```sql
WHERE a = 9
```

b-tree기반 인덱스에서는 정렬이 되어있기 때문에 이분탐색을 진행한다.

인덱스의 가운데를 고른다음, WHERE a = 9 니깐 9와 비교하면서, 이분탐색 진행

```sql
WHERE a = 7 AND b= 95
```

AND조건으로 묶였을때, 우선 a는 index기반으로 찾고, a = 7을 찾았다면, MEMBERS 테이블에서 해당 튜플로 간 다음, b 를 비교하는 방식으로 진행된다. 

a = 7인 경우가 여러개가 있을 경우가 있기에, 주변도 확인해야한다.

- WHERE a = 7은 index 기반으로 빠르게 찾을 수 있었지만, 뒤의 조건은 WHERE a = 7에서 찾은 tuple들을 full scan해야하는 상황 → 성능 좋지 않음
- 해결법은 두개를 합친 index를 만들어줘야함

```sql
CREATE INDEX(a, b)
```

![IMG_0C5F70FA5234-1](https://user-images.githubusercontent.com/69891604/215314844-ea61e951-823c-4fd7-a520-cce23e33508c.jpeg)

![IMG_CBD3CC19BDC4-1](https://user-images.githubusercontent.com/69891604/215314849-273566dc-2d4c-4c12-909c-b2de1357ffbf.jpeg)

- 따라서 이런 조건일 경우에, a 와 b를 조합한 index를 만들어주면, 성능 향상을 노려볼 수 있다.

```sql
WHERE b = 95
```

위와 같은 경우는 성능이 좋아지지 않는다, 이유는 INDEX(a,b)는 a는 정렬되어있고, b 는 전체적으로는 정렬되어있지 않기 때문이다. 이를 위해서는 b 에 대한 index를 또 만들어줘야한다.

**따라서 사용되는 쿼리에 맞춰서 적절하게 index를 걸어줘야 쿼리가 빠르게 처리될 수 있다.**

```sql
EXPLAIN
SELECT * FROM player WHERE backnumber = 7
```

DBMS의 optimizer가 알아서 적절하게 index를 선택해주는데, 직접 index를 고를 수 있다. 

이 때 사용할 수 있는 것은 `USE INDEX`, `FORCE INDEX`

```sql
SELECT * FROM player USE INDEX (backnumber_idx)
```

### index를 많이 만들어도 좋은가?

- **table에 write(INSERT, UPDATE, DELETE) 할 때마다 index 변경 발생**
    
    **그래서 인덱스가 많아지면 많아질수록 table에 write할때마다 오버헤드가 많이 발생한다.**
    
- **추가적인 저장 공간을 차지한다.**
- **그렇기 때문에 불필요한  index를 만들면 안된다.**

## Covering index

INDEX(team_id, backnumber)

| team_id | backnumber | ptr |
| --- | --- | --- |
| … | … | … |
|  |  |  |

Player

| id | name | team_id | backnumber |
| --- | --- | --- | --- |
|  |  |  |  |
|  |  |  |  |

```sql
SELECT team_id, backnumber FROM player WHERE team_id = 5
```

플레이어 테이블에서 팀아이디가 5인 팀 아이디, 백넘버를 가져올떄, index만으로도 처리가 가능하다. 이럴때 매우 효율적일수도 있음

- **조회하는 attribute를 index가 모두 cover 할 때 조회 성능이 더 빠르다.**

## Hash Index

- hash table을 사용해서 index를 구현
- 시간 복잡도 O(1)성능
- rehashing에 대한 부담(데이터가 쌓이다보면, 데이터를 쌓을 수 있는  arr에 크기를 늘려줘야하는데, 이를 rehashing이라고 한다.)
- equality 비교만 가능, range 비교 불가능 (≥와 같은 것은 불가능하다.)
- multicolumn index의 경우 전체 attributes에 대한 조회만 가능
    
    (a, b)로 index를 만들었다면,  a만 사용해서 index를 조회를 할 수 없다.
    

## Full scan이 더 좋은 경우

- table에 데이터가 조금 있을 때
- 조회하려는 데이터가 테이블의 상당 부분을 차지할 때 (여부는 optimizer가 사용)

<aside>
⭐ 1. order by나 group by에도 index 사용

2. foreign key에는 index가 자동으로 생성되지 않을 수 있다.

3. 이미 데이터가 몇 백만건 이상 있는 테이블에 인덱스를 생성하는 경우, 시간이 몇 분 이상 소요될 수 있고, DB성능에 안좋은 영향을 줄 수 있다. 특히 write에 대한 쿼리문에서는 더 성능이 안좋을 수 있다.
→ 따라서 쿼리문이 느리다고 판단될 때는 EXPLAIN을 사용하자!

</aside>

## Reference

[https://www.youtube.com/watch?v=IMDH4iAQ6zM&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=25](https://www.youtube.com/watch?v=IMDH4iAQ6zM&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=25)

## DB Index에 관련된 질문

### 1. Index가 왜 필요할까요? ( SELECT의 성능을 높일수 있는 방법이 뭐가 있을까요?)

Index는 데이터베이스에서 테이블의 검색 성능을 높여주는 대표적인 방법 중 하나입니다. 특히 조건에 만족하는 튜플을 빠르게 검색할 수 있으며, 조건에 만족하도록 정렬, 그룹핑을 빠르게 도와줍니다. 일반적인 RDBMS에서는 B+Tree 구조로 된 index를 사용해서 검색속도를 향상시킵니다.

Index가 없을때, 특정 조건에 맞는 튜플을 검색하기 위해서는 table 내 모든 튜플을 검색해야하며, 이는 O(N)의 시간복잡도를 가지며, FULL SCAN이라고 합니다. 그래서 index를 사용한다면, 이분탐색을 사용해서 O(logN)의 시간복잡도를 가집니다.

### 1-1. Index 의 내부 동작은 어떻게 동작하나요?

index를 구현하는 방법은 BTREE, B+TREE, HASH, BITMAP 등이 있지만, 대부분 B+TREE로 되어있습니다. 특정 컬럼에 대해서 인덱스를 건다면, 해당 컬럼에 맞도록 정렬되어서 새로운 물리적 공간에 별도파일로 저장됩니다. 이는 정렬된 컬럼과 ptr 컬럼을 가지며, ptr을 통해서 실제데이터로 찾아갈수 있습니다. 따라서 WHERE 조건과 일치하는 데이터들의 저장위치를 빠르게 찾을 수 있습니다.

### 1-2. Index를 많이 생성하면 안될까요?

불필요하게 많이 생성하게 되면, 추가 저장 공간이 필요하고, 데이터에 WRITE이 일어날 때마다, 관련 index를 모두 수정해야하기 때문에 오버헤드가 발생할 수 있습니다.

### 2. Index를 사용할 Column?

**index는 where 절에서 자주 조회되고, 수정 빈도가 낮으며, 카니널리티는 높고, 선택도가 낮은 column을 선택해서 설정해야한다.**

<aside>
⭐ **선택도**: 데이터에서 특정 값을 잘 골라낼 수 있는 정도 (선택도가 1이면 모든 데이터가 Unique)

**카니널리티**: 데이터가 중복되지 않은 정도 ( 주민등록번호는 카디널리티가 높으며, 성별은 카디널리티가 낮다)

</aside>

### 2-1. 회사의 고객 DB에서 성별 Column에 index를 걸어주는 것이 좋을까요?

### 2-2. true와 false 값을 갖는 column에서 true 1%, false 99% 비율로 구성된 상황에서 index를 거는 것이 좋을까요?
