### 이상현상 (Anomaly)

![스크린샷 2023-01-22 오후 11 20 38](https://user-images.githubusercontent.com/41604678/213950519-d4b820b7-1bb3-45bf-8c64-03597a6db9e1.png)   
   

1. 삽입 이상 (Insertion Anomaly) : 불필요한 정보를 함께 저장하지 않고서 어떤 정보를 저장하는 것이 불가능 할 때  
![스크린샷 2023-01-22 오후 11 20 38](https://user-images.githubusercontent.com/41604678/213950519-d4b820b7-1bb3-45bf-8c64-03597a6db9e1.png)  
강의를 수강하지 않은 학생 정보를 삽입할 때 성적은 NULL이나 불필요한 데이터 필요하다. 

2. 갱신 이상 (Modification Anomaly) : 반복된 데이터 중에 일부를 갱신할 때 데이터의 불일치가 발생  
![스크린샷 2023-01-22 오후 11 23 04](https://user-images.githubusercontent.com/41604678/213950570-f7e386f8-6b25-4165-a567-ee125dcd2823.png)  
학번이 101인 학생이 학과를 바꾸었을 때 101이 있는 모든 튜플을 수정해주어야 한다.  

3. 삭제 이상 (Deletion Anomaly) : 필요한 정보를 함께 삭제하지 않고서는 어떤 정보를 삭제하는 것이 불가능  
![스크린샷 2023-01-22 오후 11 23 43](https://user-images.githubusercontent.com/41604678/213950591-13b5633b-1ef9-4932-ab52-d671111beb19.png)  
 지도교수 교수1가 강의를 취소한다고 할때, 학번이 101인 학생에 대한 정보도 모두 삭제된다. 
