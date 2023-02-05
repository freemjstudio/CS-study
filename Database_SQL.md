## DDL (Data Definition Language)

데이터 정의어는 데이터를 정의하는 명령어이다. 데이터를 담는 그릇을 정의하는 언어라고 이해할 수 있다. 즉, TABLE과 같은 데이터 구조를 정의하는데 사용되는 명령어들이다. 
DDL의 종류로는 CREATE, ALTER, DROP, TRUNCATE 가 있다. 그리고 DDL의 대상으로는 도메인, 스키마, 테이블, 뷰, 인덱스가 있다. 

* 도메인 (Domain) : 하나의 속성이 가질 수 있는 원자값들의 집합 
* 스키마 (Schema): 데이터베이스의 구조, 제약 조건등의 정보를 담고 있는 기본적인 구조 
* 테이블 (Table) : 데이터의 저장 공간 
* 뷰 (View) : 물리 테이블에서 유도하는 가상의 테이블 
* 인덱스 (Index) : 자료검색을 빠르게 하기 위한 데이터 구조 


### 1. TABLE 관련 DDL 

#### 1) CREATE : 테이블 생성 

```sql
CREATE TABLE 테이블 명(
  컬럼이름 데이터타입 제약조건, 
  컬럼이름 데이터타입 제악조건,
  ...
);

CREATE TABLE Student(
  id VARCHAR(20) PRIMARY KEY,
  name VARCHAR(20) UNIQUE,
  birthday CHAR(8) NOT NULL, 
  sex CHAR(1) CHECK (sex ='M' OR sex ='W')
)
```

#### 2) ALTER : 테이블 수정 

* 컬럼 추가하기 (ADD)
```sql
ALTER TABLE 테이블 명 ADD 컬럼명 데이터타입 [제약조건];
ALTER TABLE Student ADD phone VARCHAR(15) UNIQUE;

```
* 컬럼 삭제하기 (DELETE)
```sql
ALTER TABLE 테이블 명 DELETE 컬럼명 데이터타입 [제약조건];
ALTER TABLE Student DELETE phone VARCHAR(15) UNIQUE;
```

* 컬럼 수정하기 (MODIFY)
```sql
ALTER TABLE 테이블 명 MODIFY 컬럼명 데이터타입 [제약조건];
ALTER TABLE Student MODIFY name VARCHAR(30) NOT NULL;
```

#### 3) DROP : 테이블 삭제 
```sql
DROP TABLE 테이블 명 [CASCADE|RESTRICT];
```

CASCADE 는 해당 테이블을 참조하는 테이블까지 연쇄적으로 삭제하는 옵션이고, RESTRICT는 다른 테이블이 삭제하려는 테이블을 참조중이면 제거하지 않는 옵션이다. 

#### 4) TRUNCATE : 테이블 내의 데이터 삭제 
```sql
TRUNCATE TABLE 테이블 명;
```
테이블 내의 모든 데이터를 삭제한다. 

### 2. VIEW 관련 DDL

#### 1) CREATE : VIEW 생성 
```sql
CREATE VIEW 뷰이름 AS 쿼리문;

CREATE VIEW 학생뷰 AS 
SELECT id, name
  FROM Student
 WHERE sex = 'F';
```
여학생들의 학번과 이름을 조회하는 뷰 생성 

#### 2) DROP : VIEW 삭제 
```sql
DROP VIEW 뷰이름;
```

### 3. INDEX 관련 DDL 
#### 1) CREATE : INDEX 생성
```sql
CREATE [UNIQUE] INDEX 인덱스명 ON 테이블명(컬럼명1, 컬럼명2, ..);
```
#### 2) ALTER : 인덱스를 수정하는 명령어 but 일부 DBMS에서는 이 기능을 제공하지 않으며, 제공된다고 하더라도 기존 인덱스를 삭제하고 다시 생성하는 방법이 권장된다. 

## DML (Data Manipulation Language) 

## DCL (Data Control Language)
