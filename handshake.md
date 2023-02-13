# 3-Way Handshake와 4-Way Handshake
<br/>

3-Way Handshake 는 TCP의 접속,4-Way Handshake는 TCP의 접속 해제 과정이다.

* 포트(PORT) 상태 정보

  CLOSED: 포트가 닫힌 상태
  
  LISTEN: 포트가 열린 상태로 연결 요청 대기 중
  
  SYN_RCV: SYNC 요청을 받고 상대방의 응답을 기다리는 중
  
  ESTABLISHED: 포트 연결 상태

* 플래그 정보

  TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
  즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
  
    - SYN(Synchronize Sequence Number) / 000010
    
    연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
    
    - ACK(Acknowledgement) / 010000
    
    응답 확인. 패킷을 받았다는 것을 의미한다.
    
    - Acknowledgement Number 필드가 유효한지를 나타낸다.
    
    양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
    
    - FIN(Finish) / 000001
    
    연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.
<br/>

## TCP의 3-Way Handshake

### 개념

   TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 연결을 설정(Connection Establish) 하는 과정
    
   양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
    
   즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.
   
   step 1 (SYN)

    클라이언트는 서버와 커넥션을 연결하기 위해 SYN을 보낸다. (seq : x)

    송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
    
    PORT 상태
        Client : CLOSED- SYN_SENT 로 변함
        Server : LISTEN
        
   Step 2 (SYN + ACK)

    서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (seq : y, ACK : x + 1)

    접속요청을 받은 Q가 요청을 수락했으며, 접속 요청 프로세스인 P도 포트를 열어달라는 메세지를 전송 (SYN-ACK signal bits set)

    ACK Number필드를 Sequence Number + 1 로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 새그먼트 전송 (Seq=y, Ack=x+1, SYN, ACK)

    PORT 상태
       Client : CLOSED
       Server : SYN_RCV

   Step 3 (ACK)

    클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄

    마지막으로 접속 요청 프로세스 P가 수락 확인을 보내 연결을 맺음 (ACK)
    
    이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.

    PORT 상태
       Client : ESTABLISED
       Server : SYN_RCV ⇒ ACK ⇒ ESTABLISED

* full-duplex 통신의 구성

  Step 1, 2에서는 P→Q 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인한다.
  
  Step 2, 3에서는 Q→P 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인한다.
  
  ⇒ 이를 통해 full-duplex 통신이 구축됩니다.
