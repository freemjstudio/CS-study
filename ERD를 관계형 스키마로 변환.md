# Reduction to Relational Schemas

E-R database schema -> relation schema
엔티티 셋과 관계 셋을 relation schemas로 표현할 수 있다.

<br/>

### 1. Strong entity Sets

    Stong Entity Set은 동일 속성을 가진 스키마로 똑같이 변환

### 2. Weak Entity Sets

    identigying strong 엔티티의 PK와 weak 엔티티의 partial key를 primary key로 스키마 구성

    couse_id는 section에서 FK가 됨.
    
### 3. Complex Attribues - Composite, multivalued, derived

    1) composite attribute는 분리된 속성으로 정의한다.

    엔티티 셋이 다음과 같으면 관계형 스키마는 instructor(ID, first_name, middle_initial, last_name)으로 정의
    
    2) multivalued attribute는 분리된 스키마로 정의하고 foreign key로 연결한다.
    
        * special case : 한 엔티티 집합이 PK와 multivalued 속성만 갖는 경우엔 분리된 스키마를 정의하지 않고 한 번에 정의
    
    3) derived attribute는 따로 정의할 필요 없다.

### 4. Relationship Set

    관계 셋의 스키마는 연결된 두 엔티티의 PK를 속성으로 정의된다. 정의된 스키마에서 PK를 뭘로 사용할 지는 집합 관계마다 다르다.
    
    one-to-one 관계 : 두 엔티티의 PK 중 아무거나 하나만 PK로 사용
    many-to-many 관계 : 두 엔티티의 PK를 모두 PK로 사용함.
    one-to-many / many-to-one 관계 : many쪽의 PK를 PK로 사용함
