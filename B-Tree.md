# B Tree 
![b-tree-binary-tree](https://user-images.githubusercontent.com/41604678/215308222-a3e0c41c-c2e6-4f2c-85a1-f06876315ede.png)

- B Tree는 Binary Tree와 유사한 구조를 가지지만, 한 node 당 자식 node를 2개 이상 가질 수 있다. 
- 특정 값을 탐색하기 위해 root node 에서부터 leaf node까지 key 값을 이용하여 찾아 들어간다. 
- O(logN) 시간 복잡도를 가지므로 트리 구조 특성 상 단시간 내에 탐색 가능하다. 

# B+Tree
- B Tree 자료구조를 활용하는 확장된 자료구조형이다. 
- MySQL의 DB Engine인 InnoDB는 B+Tree 자료구조를 사용한다. 


*  Node Structure 

B+Tree 자료 특징 
- root node 에서 leaf node가는 path 는 모두 같은 길이를 가진다. 
- 
