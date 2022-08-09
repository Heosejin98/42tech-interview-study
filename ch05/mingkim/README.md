# [TCP] 3 way handshake & 4 way handshake

---

[참고 링크](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake)  
[참고 링크](https://bangu4.tistory.com/74)
[참고 링크](https://www.geeksforgeeks.org/tcp-connection-termination/)

---

> 연결을 성립하고 해제하는 과정을 말한다.

<br />

## 3 way handshake

TCP는 장치들 사이에 논리적인 접속을 성립시키기 위해 3 way handshake를 사용한다.

TCP 3 way handshake(이하 3way)는 TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이

데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 `상대방 컴퓨터와 사전에 세션을 수립하는 과정`을 의미한다.

TCP 통신은 PAR(Positive Acknowledgement with Re-transmission)을 통해 신뢰적인 통신을 제공한다.

PAR을 사용하는 기기는 ack을 받을 때까지 데이터 유닛을 재전송한다.

수신자가 데이터 유닛(세그먼트)이 손상된 것을 확인하면 해당 세그먼트를 없앤다.

그러면 sender는 positive ack이 오지 않은 데이터 유닛을 다시 보내야 한다.

### 작동방식

HOST P와 HOST Q가 있을 때, _`HOST P가 클라이언트`_, _`HOST Q가 서버`_ 라고 가정해보자.

<br />

<img src="https://camo.githubusercontent.com/4acea6af95884347810f057d00c6c4643a56d4a7dbbdf49740745560cd45cc1f/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f5443502d636f6e6e656374696f6e2d312e706e67" width=500 height=500>

<br />

1. 클라이언트가 서버에게 SYN 패킷을 보냄.  
   Client > Server : TCP SYN
2. 서버가 SYN(x)을 받고 클라이언트로 받았다는 신호 ACK(x + 1)와 SYN(y)패킷을 보냄.  
   Server > Client : TCP SYN, ACK
3. 클라이언트는 서버의 응답(ACK(x+1), SYN(y))을 받고, ACK(y+1)를 서버로 보냄.  
   Client > Server : TCP ACK

- SYN : 'Synchronize sequence numbers', 연결 요청. 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 보냄.
- ACK : 'Acknowledgement', 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송.

> 동기화 요청에 대한 답변: `Client의 Sequence Number + 1`을 하여 ACK로 돌려준다.

이렇게 3번의 통신이 완료되면 연결이 성립된다.

SYN을 보내면 ACK를 받을 때 까지 재전송한다. ACK가 와야 완료된다는 뜻이다.

이러한 행위로 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고,

실제로 데이터 전달이 시작하기 전에 한쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.

---

## 4 way handshake

4 way handshake는 연결을 해제하는 과정이다. 여기서는 FIN 플래그를 이용한다.

- FIN : 세션을 종료시키는데 사용되며, 더 이상 보낸 데이터가 없음을 나타낸다.

### Termination의 종류

TCP는 두 가지 연결 해제 방식이 있다.

1. Graceful connection release(정상적인 연결 해제)

정상적인 연결 해제에서는 양쪽이 커넥션을 모두 닫을 때까지 연결되어 있다.

2. Abrupt connection release(갑작스런 연결 해제)
   1. 갑자기 한 TCP 엔티티가 연결을 강제로 닫는 경우
   2. 한 사용자가 두 데이터 전송 방향을 모두 닫는 경우

#### Abrupt

RST(TCP reset) 세그먼트가 전송되면 갑작스러운 연결 해제가 수행되는데, RST 세그먼트는 다음과 같은 경우에 전송된다.

1. 존재하지 않는 TCP 연결에 대해 비 SYN 세그먼트가 수신된 경우
2. 열린 커넥션에서 일부 TCP 구현이 잘못된 헤더가 있는 세그먼트가 수신된 경우
   - RST 세그먼트를 보내, 해당 커넥션을 닫아 공격을 방지한다.
3. 일부 구현에서 기존 TCP 연결을 종료해야 하는 경우

#### Graceful

연결 종료 요청 역시, 요청을 먼저 시도한 요청자를 Client로, 요청을 받은 수신자를 Server 쪽으로 생각하면 된다.

<br />

<img src=https://camo.githubusercontent.com/8bb8960e46a3bfada6a237a7a91bce75a0a3e0e34eab5c1f5143ca6fe34d0b5f/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f434e2e706e67 width=500 height=500 />

<br />

1. 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그 보냄.
2. 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보낸다. 이 때, 모든 데이터를 보내기 위해 CLOSE_WAIT 상태가 된다.
3. 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보낸다.
4. 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다. 이 때 아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT을 통해 기다린다.
   1. 서버는 ACK를 받고, 소켓을 닫는다(Closed)
   2. TIME_WAIT 시간이 끝나면 클라이언트도 닫는다 (Closed) => 의도치 않은 에러로 연결이 데드락으로 빠지는 것을 방지한다.

이렇게 4번의 통신이 완료되면 연결이 해제된다.