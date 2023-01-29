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

### B+Tree 자료 특징 


- root node 에서 leaf node가는 path 는 모두 같은 길이를 가진다. 
- root node 는 반드시 2개 이상의 children node 를 가져야 한다. 
- leaf node는 반드시 (n-1)/2 ~ (n-1) 개의 value를 가져야 한다. 
위의 예시에서는 n=6 이므로 leaf node는 반드시 3개이상 5개 이하의 value를 가져야 한다.  
- Root node가 아닌 Non-leaf node 는 반드시 (n/2) ~ n 개의 children node를 가져야 한다. 
위의 예시에서는 3 개이상 6개 이하의 자식 노드를 가져야 한다. 

### B+Tree Insertion 

### B+Tree Deletion 

### B+Tree 
