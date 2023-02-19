# TCP 프로토콜의 혼잡제어 (Congestion Control)

TCP 는 우선 reliable network 를 보장하는 프로토콜이다. 
reliable network 를 보장하는 것은 4가지 문제점이 있다. 
1) Packet의 손실 문제 
2) Packet의 순서가 바뀌는 문제 
3) 네트워크의 혼잡 문제 ---> Congestion Control
4) Receiver 의 buffer가 overload되는 문제 ---> Flow Control 

이 문제들 중, 3) 에 해당하는 네트워크 혼잡 문제를 해결하기 위해 송신측 데이터 전달과 네트워크 데이터 처리 속도의 차이를 해결하기 위한 방법이 혼잡제어이다. 

<img src="https://user-images.githubusercontent.com/41604678/219958762-518b8e79-3614-4291-995f-6df493446ed1.png" width="800">

* x축 : time   
* y축 : congestion window size   


혼잡제어는 3가지 main phase로 구성되어 있다.   

### 1. Slow Start
전송 속도를 천천히 늘려가며 네트워크 혼잡 상태를 확인한다. Slow Start는 CongWin이 Threshold 에 도달하기 이전까지 진행된다. 천천히 데이터 양을 늘려가는 이유는 현재 네트워크의 상태를 알지 못하기 때문이다. Slow Start의 단점은 전송 속도를 늘리는데 너무 오래걸린다는 점이다. 이를 해결하기 위해서 패킷이 잘 도착했을 때 그 다음 패킷을 전송할 때에는 window size 를 2배씩 늘린다. 이때 데이터를 보내는 양의 단위는 MSS (Maximum Segment Size)이며, 500Byte이다. 즉, 맨 처음 혼잡제어에 따라 slow start를 시작할 때의 window size는 1 MSS라고 이해할 수 있다. 그리고 전송이 잘 되었다는 의미로 ACK를 받으면 window size가 2배가 되는 것이다. 


### 2. Additive Increase
Congestion Window Size가 Threshold 지점을 넘어서게 되면, Additive Increase 구간에 도달한다. 이는 linear하게 window size를 늘려가는 것이다. 

### 3. Multiplicative Decrease
Additive Increase 구간에서 Pakcet Loss가 발생하면 congestion window size를 다시 1 MSS로 줄이고, Threshold지점은 Packet Loss가 일어난 지점의 Congestion Window Size의 1/2 지점으로 재설정한다. 

### TCP Series 1 Tahoe vs TCP Series 2 Reno 의 비교 
TCP Series 2 Reno는 TCP Series 1 Tahoe의 혼잡제어 문제점을 개선했다. 
TCP Series 1 Tahoe는 Packet Loss가 발생한 시점을 기준으로 window size를 1 MSS로 급격하게 줄이고 있다. 그런데 Packet Loss가 검출되는 경우는 2가지가 있다. 

1) Time Out : 정해진 시간 동안 아무런 패킷을 받지 못한 상태
2) 3 duplicate ACK : 동일한 ACK 넘버를 3번 이상 받게 되어 패킷이 제대로 보내지지 않은 상태. 송신측에서는 패킷 재전송을 한다. 

그런데 3 duplicate ACK는 패킷이 오고 가는 상태이지만 Time Out은 아무것도 받지 못하는 상태로, Time Out이 발생하는 상황의 네트워크 상태가 더 혼잡하다고 간주할 수 있다. 기존의 TCP Series 1 Tahoe에서는 이 두가지 경우에 대한 차이를 두지 않았으나, Reno에서는 Time Out이 발생한 경우에만 window size 를 1MSS로 급격하게 줄이고 있다. 반면 3 duplicate ACK 에 의해 발견된 Packet Loss 상황에서는 window size를 1/2줄인 후 Additive Increase를 시작한다. 두가지 다 threshold는 Packet Loss가 발생한 시점의 window size 크기의 1/2로 줄이는 것은 동일하다. 


Q. 왜 데이터 전송량을 늘릴때는 linear 하게 늘리고 threshold에 도달해서 전송량을 줄일 때는 급격하게 줄이는가?
A. 네트워크는 공유자원이기 때문이다. 네트워크가 혼잡상태여서 막히게 되면 전송 문제가 해결되지 않는다. 따라서 혼잡 문제를 해결하기 위해서 전송량을 급격히 줄이는 것이며, 혼잡 상태를 방지하기 위하여 전송량을 linear하게 늘려가는 것이다. 

Q. 왜 최대로 데이터 량을 많이 보낼 수 있는 지점을 찾아서 전송량을 고정적으로 보내지 않는가?
A. 네트워크라는 것은 공유 자원이며, 네트워크 상태에 따라 데이터 량을 최대로 많이 보낼 수 있는 지점이 계속 바뀌게 된다. 또한 packet loss 가 발생하는 지점에 도달한 이후에는 네트워크 속도가 느려지게 될 것이므로 혼잡제어 과정을 반복할 수 밖에 없는 것이다. 

* 참고 : 맨 처음 theshold는 어떻게 지정해 주는 것일까 ? -> 사실 맨 처음, 이전의 네트워크 상태에 대한 정보 없이 threshold가 어디인지는 알 수 없다. 직접 데이터를 전송해 보며 packet loss 상태에 도달하기 전까지 확인하거나 threshold를 임의로 지정해주게 될 것이다. 


### 데이터 전송 속도 
전송 속도를 수식으로 표현하면 다음과 같다. 

<img src="https://user-images.githubusercontent.com/41604678/219958765-d72cca74-2ee4-44ee-ac9c-b6d0d634ecb9.png" width="500">  

전송 속도의 단위는 bps(Bytes per Sec) 이고, CongWin 는 Congestion Window 의 사이즈, RTT는 Round Trip Time으로 송신지부터 목적지까지 패킷이 왕복하는 시간이다. 
각각의 요소 중에서는 실제로는 Congestion Window 의 변화가 많다. 따라서, 데이터의 전송 속도(rate)는 Congestion Window의 사이즈에 따라 좌우된다. 그리고 이 Congestion Window의 사이즈를 결정하는 것은 네트워크의 상태이다. 네트워크의 상태가 혼잡하지 않다면, Congestion Window의 사이즈를 늘릴것이기 때문이다. 
즉, 네트워크가 전송속도를 결정한다.
