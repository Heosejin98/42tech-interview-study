# UDP (User Datagram Protocol)

TCP(Transmission Control Protocol)과 함께 Transport Layer에서 사용되는 통신규약.

## 개발배경

기존에 사용하던 TCP의 경우 신뢰성은 보장할 수 있으나 실시간 스트리밍 혹은 게임 서버처럼 실시간성이 중요하게 여겨지는 서비스에서 일부 패킷의 누락으로 버퍼링이 걸리거나 랙이 걸리는 일이 많았다고함. 이러한 현상을 해결하기 위해 도임된 것이 UDP임

## 패킷의 단위 - Message

![UPD header](https://i2.wp.com/ipwithease.com/wp-content/uploads/2018/01/091-udp-user-datagram-protocol-01.png?w=607&ssl=1)

TCP의 패킷 단위인 Fragment는 각 fragment 사이에 순서라는 관계가 있지만  독립적인 데이터 덩어리임

## UDP의 특징
TCP의 모든 신뢰기능이 없음
- 비연결성 - handshaking 과정이 없기 때문에 연결성을 가지지 않음
- 순서제어 X, 흐름제어 X, 확인응답 X(송신측에서)
- 오류제어 거의 없음
    - check sum이 있지만 제어는 따로 하지 않음, 따라서 application layer에서 이를 처리함(?)
    - 성능을 위해서 check_sum을 0으로 할 경우 수신측에서 check_sum 계산도 하지 않음
- 1:1, 1:M, N:M 전송 가능
- 전송속도 제한 없음(?) - 아마도 흐름제어를 하지 않기 때문이라고 생각함.
- 헤더가 단순함 - Fixed 8 Byte

## UDP 패킷의 여행

![Message path](https://t1.daumcdn.net/cfile/tistory/9969973359FEB59309?download)


UDP 위에서 동작되는 다양한 프로토콜 또는 응용분야
- TFTP, SNMP, DHCP, NFS, DNS, RIP, NTP, RTP


참고
- (https://cheapsslsecurity.com/blog/tcp-vs-udp-how-are-they-different/)
- (https://mangkyu.tistory.com/15)