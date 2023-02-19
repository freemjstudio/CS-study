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
#### Additive Increase
#### Multiplicative Decrease


![image](https://user-images.githubusercontent.com/41604678/219958765-d72cca74-2ee4-44ee-ac9c-b6d0d634ecb9.png)
