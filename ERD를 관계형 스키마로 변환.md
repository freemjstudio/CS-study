# Reduction to Relational Schemas

E-R database schema -> relation schema
엔티티 셋과 관계 셋을 relation schemas로 표현할 수 있다.

<br/>


![ddw](https://user-images.githubusercontent.com/42952074/215301234-2280fe36-2ac7-4e15-9133-1e3a064f6156.png)

<br/>


* Participation Constrains ( ERD 볼때 참고 )
    
![img1 daumcdn](https://user-images.githubusercontent.com/42952074/215305074-a7904963-824f-4003-83f8-7fd356953a7e.png)

        
        선이 하나이면 partial 참여, 선이 두 개면 total 참여를 의미한다.

        선 표현 대신 cardinality limits 표현으로 participation constrain을 의미할 수도 있음
        
![img1 daumcdn](https://user-images.githubusercontent.com/42952074/215305097-15f9b736-286c-4148-95cd-0a417d58205d.png)
        
        0..*에서 최소 0번이니까 모든 엔티티가 참여하는 것은 아님 -> partial

        1..1에서 모든 엔티티가 한 번씩 참여하니까 -> total

        1..*은 최소 1번에서 N번 참여하는 것이므로 -> total

* Weak entitiy sets ( ERD 볼때 참고 )

![img1 daumcdn](https://user-images.githubusercontent.com/42952074/215305211-61fa5c53-c7bf-4509-9ce6-518198443acc.png)

        Weak Entity Set : primary key가 없는 엔티티 셋을 의미한다.

        Strong Entity Set : primary key를 갖는 엔티티 셋.

        weak 엔티티 셋은 다른 strong 엔티티 셋에 의존한다. (그 strong 엔티티 셋을 identifying entity set이라 함)

        이 두 엔티티 셋 간의 관계를 identifying relationship 이라하고 double diamond로 표현한다.

        discriminator(partial key) : 
        weak 엔티티에서 점선 밑줄 그어진 속성. 나중에 section의 primary key를 만들 때 course_id와 함께 primary key를 구성하게 됨.
        
            ex) 여기서 section(분반)은 과목이 있어야 개설가능하기 때문에 weak entitiy로 설정한 것이다.


### 1. Strong entity Sets

    Stong Entity Set은 동일 속성을 가진 스키마로 똑같이 변환
    
    ex) department, instructor, student, course, classroom
    
        * special case : 다중값 속성을 가진 Strong entitiy
        
        ex) time_slot
   

### 2. Weak Entity Sets

    < identifying strong 엔티티의 PK  +  weak 엔티티의 partial key = primary key > 로 스키마 구성

    strong 엔티티의 PK를 가져온 것이므로 해당 속성은 FK가 된다.
    
        ex) couse_id는 section에서 FK가 됨.
        
    또한 둘 사이의 관계인 identifying relationship 은 스키마를 만들지 아니한다.
    
<br/>
<img src="https://user-images.githubusercontent.com/42952074/215302118-fb5fe708-1723-44d3-a34d-4ffed30de3fc.png" width="500" height="400"/>
<br/>

### 3. Complex Attribues - Composite, multivalued, derived
<br/>
<img src="https://user-images.githubusercontent.com/42952074/215302637-2c8888e8-fd05-4acf-8ace-8775d5e8db6f.png" width="200" height="400"/>
<br/>

![img1 daumcdn](https://user-images.githubusercontent.com/42952074/215302769-41f83fef-32a3-4813-911f-a18717de4faa.png)
![img1 daumcdn](https://user-images.githubusercontent.com/42952074/215302772-6ec9ad41-78eb-4909-af66-7bed9da6362f.png)



    1) composite attribute는 분리된 속성으로 정의한다.

        composite attributes에서 leaf node만 flatten되어 나온다.
    
    2) multivalued attribute는 분리된 스키마로 정의하고 foreign key로 연결한다.
    
        ex) 다중값인 phone_number는 column으로 추가하면 atomic한 value로 구성될 수 없기 때문에 따로 테이블을 만든다.
    
    3) derived attribute는 따로 정의할 필요 없다.
    


### 4. Relationship Set

    관계 셋또한 스키마를 구성한다. 관계 셋의 스키마는 연결된 두 엔티티의 PK를 속성으로 정의된다. 
    
    정의된 스키마에서 PK를 뭘로 사용할 지는 집합 관계마다 다르다.
    
    one-to-one 관계 : 두 엔티티의 PK 중 아무거나 하나만 PK로 사용
    many-to-many 관계 : 두 엔티티의 PK를 모두 PK로 사용함.
    one-to-many / many-to-one 관계 : many쪽의 PK를 PK로 사용함
    
<br/>    

![img1 daumcdn](https://user-images.githubusercontent.com/42952074/215303760-a29f86fe-69e7-4d91-a1f2-fe0b90301652.png)


    하지만 모든 relation이 table로 변환되는 것은 아니며, 다대일 또는 일대일일 경우 아래처럼 테이블을 만들지 않고 redundancy를 줄일 수 있다.
    다대일일 경우, 다쪽에 추가해야하고 / 일대일일 경우 어느 쪽에 추가하던지 상관없다.
   
    ex) 위의 방식대로 변환한 경우,
   
    dept(dept_name, building, budget) - dept_name 이 pk
    instructor(id, name, salary) - id
    inst_dept(id, dept_name) - id

    하지만 이 경우 중복이 생기니까 아래처럼 "many, 다" side쪽에 속성을 하나 추가(extra attribute)해서 아래처럼 변환이 가능하다.

    dept(dept_name, building, budget)
    instructor(id, name, salary, dept_name)
    
    instructor에 dept_name을 추가해서 외래키로 설정하면 inst_dept가 없어도 무방하다.

<br/>

![img1 daumcdn](https://user-images.githubusercontent.com/42952074/215304014-9f49d23a-223c-48c7-b2c5-9e3f8ac16345.png)


    하지만 이런식으로 redundancy를 줄이는 방법에도 문제가 있다. 
    
    위의 예시는 total participation으로 교수가 반드시 과가 정해지기 때문에 dept_name을 속성으로 추가했을 때 null이 들어갈 염려가 없지만,
    partial participation의 경우는 dept_name column에 null이 많이 생길 수 있다.
    
    null이 많은 데이터베이스가 좋은 형태는 아니기 때문에 꼭 table을 줄이는게 최선은 아니다.
    
  
  <br/>
  그렇다면 맨 위의 ERD를 RDB 스키마로 변환하였을 경우, 몇개의 테이블이 나올수 있을까?
