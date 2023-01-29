# B Tree 
![b-tree-binary-tree](https://user-images.githubusercontent.com/41604678/215308222-a3e0c41c-c2e6-4f2c-85a1-f06876315ede.png)

- B Tree는 Binary Tree와 유사한 구조를 가지지만, 한 node 당 자식 node를 2개 이상 가질 수 있다. 
- 특정 값을 탐색하기 위해 root node 에서부터 leaf node까지 key 값을 이용하여 찾아 들어간다. 
- O(logN) 시간 복잡도를 가지므로 트리 구조 특성 상 단시간 내에 탐색 가능하다. 

  
# B+Tree

![image](https://user-images.githubusercontent.com/41604678/215308280-ce764366-fc0c-4514-b962-116d68c19281.png)

- B Tree 자료구조를 활용하는 확장된 자료구조형이다. 
- MySQL의 DB Engine인 InnoDB는 B+Tree 자료구조를 사용한다. 


### Node Structure   

![image](https://user-images.githubusercontent.com/41604678/215308290-d08de45d-f672-42ce-b701-119ae17c21a0.png)
- Ki 는 search-key value를 나타낸다. 
- Pi 는 non-leaf node의 경우 child node 를 가리키는 pointer이며, leaf node인 경우 records 또는 record bucket 을 가리키는 pointer이다. 
- Pn 은 linked list 처럼 next node를 가리킨다. 

### B+Tree 자료 특징 

![image](https://user-images.githubusercontent.com/41604678/215308857-f286205f-3375-4bd3-a649-6fa0cf027be8.png)

- root node 에서 leaf node가는 path 는 모두 같은 길이를 가진다. 
- root node 는 반드시 2개 이상의 children node 를 가져야 한다. 
- leaf node는 반드시 (n-1)/2 ~ (n-1) 개의 value를 가져야 한다. 
위의 예시에서는 n=6 이므로 leaf node는 반드시 3개이상 5개 이하의 value를 가져야 한다.  
- Root node가 아닌 Non-leaf node 는 반드시 (n/2) ~ n 개의 children node를 가져야 한다. 
위의 예시에서는 3 개이상 6개 이하의 자식 노드를 가져야 한다. 
  
### B+Tree Insertion 

- Before Insertion of 'Adams'  

![image](https://user-images.githubusercontent.com/41604678/215308870-7ea881e2-0799-45fb-af0a-dd2d9e0c1063.png)  


- After Insertion of 'Adams'
![image](https://user-images.githubusercontent.com/41604678/215308884-d5a65524-c15d-45bc-9cb9-6d29e7ed8d72.png)
  
 (search-key-value, pointer) 쌍을 index b+tree에 삽입하는 경우이다. 부모 노드가 가득 차 있는 상태이면, 부모 노드를 split 한다.   
 위의 예시에서 Adams가 삽입되어야 할 노드에 이미 값이 가득 차 있는 상태여서, 'Califieri'와 'Crick'를 split하고 이 둘을 새로운 Node에 추가한다.   
 또한 이 새로운 node를 찾을 수 있도록 하기 위해서 Parents 노드에 califieri 를 업데이트 해 준다.   
   
   
### B+Tree Deletion 
- Before Deletion of 'Srinivasan'
![image](https://user-images.githubusercontent.com/41604678/215308965-a482016f-0755-4d4c-83b4-bdebb7974f56.png)

- After Deletion of 'Srinivasan'
![image](https://user-images.githubusercontent.com/41604678/215308915-90e75b19-4734-4b84-936c-b7d7f2caa396.png)

(search-key-value, pointer) 쌍을 b+tree에서 삭제하는 경우이다. (search-key-value, pointer)를 leaf node에서 삭제해야 한다. 단, 삭제 이후에 node가 너무 적은 entries를 가지게 되면, 이웃한 노드와 merge한다.  
* leaf node는 반드시 (n-1)/2 ~ (n-1) 개의 value를 가져야 한다 라는 특성을 참고 ! 
  
  
### B+Tree File Organization 

![image](https://user-images.githubusercontent.com/41604678/215308991-a3fc4571-214f-4afd-a1d3-688fe7ea5f3e.png)

- B+Tree File Organization 에서 leaf node들은 pointer 대신 record를 저장한다. 
- 삽입/삭제/변경이 일어나도 record 데이터들을 clustered 된 상태로 유지한다. 
- leaf node들은 절반 이상 데이터들로 차 있어야 한다. (half-full)
- B+Tree Index와 유사하게 동작한다. 

  
    
--------
자료 출처 : Database Concepts 7th Edition 
