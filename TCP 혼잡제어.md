# TCP 프로토콜의 혼잡제어 (Congestion Control)

TCP 는 우선 reliable network 를 보장하는 프로토콜이다. 
reliable network 를 보장하는 것은 4가지 문제점이 있다. 
1) Packet의 손실 문제 
2) Packet의 순서가 바뀌는 문제 
3) 네트워크의 혼잡 문제 ---> Congestion Control
4) Receiver 의 buffer가 overload되는 문제 ---> Flow Control 

이 문제들 중, 3) 에 해당하는 네트워크 혼잡 문제를 해결하기 위해 송신측 데이터 전달과 네트워크 데이터 처리 속도의 차이를 해결하기 위한 방법이 혼잡제어이다. 

![image](https://user-images.githubusercontent.com/41604678/219958762-518b8e79-3614-4291-995f-6df493446ed1.png)

혼잡제어는 3가지 main phase로 구성되어 있다. 

#### Slow Start
전송 속도를 천천히 늘려가며 네트워크 혼잡 상태를 확인한다. 천천히 데이터 양을 늘려가는 이유는 현재 네트워크의 상태를 알지 못하기 때문이다. Slow Start의 단점은 전송 속도를 늘리는데 너무 오래걸린다는 점이다. 이를 해결하기 위해서 패킷이 잘 도착했을 때 그 다음 패킷을 전송할 때에는 window size 를 2배씩 늘린다. 이때 데이터를 보내는 양의 단위는 MSS (Maximum Segment Size)이며, 500Byte이다. 맨 처음 혼잡제어에 따라 slow start를 시작할 때의 window size는 1 MSS라고 이해할 수 있다. 그리고 전송이 잘 되었다는 의미로 ACK를 받으면 window size가 2배가 되는 것이다. 

#### Additive Increase

#### Multiplicative Decrease


Q. 왜 데이터 전송량을 늘릴때는 linear 하게 늘리고 threshold에 도달해서 전송량을 줄일 때는 급격하게 줄이는가?
A. 네트워크는 공유자원이기 때문이다. 네트워크가 혼잡상태여서 막히게 되면 전송 문제가 해결되지 않는다. 따라서 혼잡 문제를 해결하기 위해서 전송량을 급격히 줄이는 것이며, 혼잡 상태를 방지하기 위하여 전송량을 linear하게 늘려가는 것이다. 


#### 데이터 전송 속도 
전송 속도를 수식으로 표현하면 다음과 같다. 

![image](https://user-images.githubusercontent.com/41604678/219958765-d72cca74-2ee4-44ee-ac9c-b6d0d634ecb9.png)
전송 속도의 단위는 bps(Bytes per Sec) 이고, CongWin 는 Congestion Window 의 사이즈, RTT는 Round Trip Time으로 송신지부터 목적지까지 패킷이 왕복하는 시간이다. 
각각의 요소 중에서는 실제로는 Congestion Window 의 변화가 많다. 따라서, 데이터의 전송 속도(rate)는 Congestion Window의 사이즈에 따라 좌우된다. 그리고 이 Congestion Window의 사이즈를 결정하는 것은 네트워크의 상태이다. 네트워크의 상태가 혼잡하지 않다면, Congestion Window의 사이즈를 늘릴것이기 때문이다. 
즉, 네트워크가 전송속도를 결정한다.
