# IPC(Inter-Process Communication) - 프로세스 간 통신

IPC는 프로세스들 사이에 데이터를 주고 받는 행위 또는 그에 대한 방법이나 경로를 뜻한다.
<!-- 
IPC는 마이크로 커널 나노커널의 디자인 프로세스에 매우 중요하다: 마이크로커널은 커널이 제공하는 기능 수를 줄여준다. 해당 기능들은 IPC를 통해 서버와 통신함으로써 얻으며 일반적인 모놀리딕 커널에 비해 IPC수가 극적으로 증가된다.

![kernel](https://upload.wikimedia.org/wikipedia/commons/thumb/6/67/OS-structure.svg/1280px-OS-structure.svg.png)

* 마이크로커널: 운영 체제에 추가되어야 하는 매커니즘을 최소한으로 제공하는 초소용 커널 -->

## IPC
||||||||
| --- | --- | --- | --- | --- | --- | --- |
| IPC 종류 | PIPE | Named PIPE | Message Queue | Shared Memory | Memory Map | Socket |
| 용도 | 부모 자식 간 단방향 통신 | 다른 프로세스와 단방향 통신 | 다른 프로세스와 단방향 통신 | 다른 프로세스와 양방향 통신 | 다른 프로세스와 양방향 통신 | 다른 시스템과 양방향 통신 |
| 매개체 | 파일 | 파일 | 메모리 | 메모리 | 파일 + 메모리 | 소켓 |
| 통신 단위 | stream | stream | 구조체 | 구조체 | 페이지 | stream |
| 통신 가능 범위 | 동일 시스템 | " | " | " | " | 동일 + 외부 시스템 |
| 운영 체제 | 대부분의 운영 체제 | 모든 POSIX 시스템, 윈도 | 대부분의 운영 체제 | 모든 POSIX 시스템, 윈도 | " | 대부분의 운영 체제 |


## IPC별 세부 설명

### PIPE
1. 정의
- '익명'의 PIPE를 통해서 동일한 PPID(부모 프로세스 ID)를 가진 프로세스들 간에 단방향 통신을 지원
2. 구조
- FIFO 구조
- 생성된 PIPE에 대하여 Write 또는 Read만 가능
3. 사용 시기
- 부모 자식 프로세스간 통신 할때 사용 (그럼 자식끼리는 안되나?)
4. 유의사항
- 쌍방 통신을 위해서는 Write용, Read용 각각 하나씩 pipe를 하나씩 만들어야함
- read()와 write()가 기본적으로 block 모드로 작동하기 때문에 프로세스가 read대기중이라면 read가 끝나기 전에는 write를 할 수가 없게 됨
![pipe](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F99D729355B9725A83FFBE5)

*pipe란 결국 한 파일에 대하 read only로 연 fd하나와 write only로 연 fd하나를 생성해주는? 그거인듯

### Named PIPE
1. 정의
- 이름을 가진 PIPE를 통해서 프로세스들 간에 단방향 통신을 지원
- 서로 다른 프로세스들이 PIPE의 이름만 알면 통신이 가능하다.
2. 구조
- PIPE와 같음
3. 사용 시기
- 연관이 전혀 없는 프로세스간 통신을 할때
4. 유의사항
- PIPE와 같음
![named pipe](https://t1.daumcdn.net/cfile/tistory/998430355B9725A813?download)

* mkfifo를 통해 file 명을 지정할 수 있다.

### Message Queue
1. 정의
- 메모리를 사용한 PIPE
- 구조체 기반으로 통신을 함
2. 구조
- FIFO
- msgtype에 따라 다른 구조체를 가져올 수 있다.
3. 사용 시기
- 프로세스 간 다양한 통신을 할 때 사용할 수 있다.
4. 유의사항
- 커널에서 제공하는 Message Queue이기 때문에 EnQueue하는데 제한이 존재
![message queue](https://t1.daumcdn.net/cfile/tistory/9936B1335B975B8105?download)

* 파일이 아니라 커널의 메모리 상에서 관리됨

### Shared Memory
1. 정의
- 시스템 상의 공유 메모리를 통해 통신한다.
2. 구조
- 일정 크기의 메모리를 프로세스간에 공유하는 구조
- 공유 메모리는 커널에서 관리
3. 사용 시기
- 프로세스 간 Read, Write가 모두 필요할 때
4. 유의사항
- 프로세스
![shared memory](https://t1.daumcdn.net/cfile/tistory/995C864E5B9766171D?download)


### Memory Map
1. 정의
- 파일을 프로세스의 메모리에 일정 부분 맵핑시켜서 사용한다.
2. 구조
3. 사용 시기
- 파일로 대용량 데이터를 공유할 때 사용
- FILE IO가 느릴 떄 사용
4. 유의사항
![memory map](https://t1.daumcdn.net/cfile/tistory/9951304F5B9863F82D?download)

* 가상 메모리의 page를 사용한다는데 더 공부

### Socket
1. 정의
- 네트워크 소켓통신을 사용한 데이터 공유
2. 구조
- 네트워크 소켓을 이용하여 Client-Server 구조로 데이터 통신
3. 사용 시기
- 원격에서 프로세스간 데이터를 공유할 때 사용
4. 유의사항
![socket](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile22.uf.tistory.com%2Fimage%2F99F640335B986BCD2DE4BC)
* https://hyeon9mak.github.io/socket-overview/

### Signal
1. 정의
2. 구조
3. 사용 시기
4. 유의사항
![]()

### Semaphore
Semaphore는 Named PIPE, PIPE, Message Queue와 같은 다른 IPC설비들이 대부분 프로세스간 메시지 전송을 목적으로 하는데 반해, Semaphore는 프로세스 간 데이터를 동기화 하고 보호하는데 그 목적을 두게 됩니다.

* 참고
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tlqdnjsahwk&logNo=220025574804
https://velog.io/@jhlee508/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-Message-Queue-IPC
https://movefast.tistory.com/344