# TCP vs UDP

### TCP vs UDP

---

**개념**

- TCP(Transmission Control Protocol)과 UDP(User Datagram Protocol)는 OSI 7계층 중 **전송 계층(Transport Layer)** 에서 동작하는 대표적인 두 프로토콜이다.
- 둘 다 “포트 번호”를 사용해 어플리케이션 간 데이터를 전달하지만, 전송 신뢰성, 순서 보장, 연결 방식 등에서 큰 차이를 보인다.

> 간단히 말하면,
**TCP**는 신뢰성과  순서를 보장하는 **연결형 프로토콜**,
**UDP**는 빠른 전송을 위한 **비연결형 프로토콜** 이다.
> 

### TCP (Transmission Control Protocol)

---

- **연결형(Connection-oriented) 프로토콜**
    
    → 통신을 시작하기 전에 **3-way handshake** 과정을 통해 논리적 연결을 수립함.
    
- 신뢰성 있는 데이터 전송 보장
    - 패킷 손실 시 재전송
    - 순서 보장(Segment 번호 기반)
    - 흐름 제어(Flow Control) : 송수신 속도 조절
    - 혼잡 제어(Congestion Control) : 네트워크 혼잡 완화
- 스트림(Stream) 기반 : 바이트 단위의 연속적인 데이터 전송
- 대표 사용 예시
    - 웹(HTTP, HTTPS)
    - 이메일(SMTP, IMAP, POP3)
    - 파일 전송(FTP, SFTP)

<aside>
💡

**3-Way Handshake (연결 성립)**

![3way_handshake.png](3way_handshake.png)

1. Client → Server : SYN (연결 요청)
2. Server → Client : SYN + ACK (요청 수락)
3. Client → Server : ACK (연결 확인)

이후 실제 데이터 전송 시작

연결 종료 시에는 **4-Way Handshake** 로 세션을 안전하게 닫음

</aside>

### UDP(User Datagram Protocol)

---

- **비연결형(Connectionless) 프로토콜**
    
    → 연결 절차 없이 데이터를 즉시 전송(Handshake 없음)
    
- 비신뢰성 (Unreliable)
    - 패킷 손실 시 재전송하지 않음
    - 순서 보장 X, 수신 여부 확인 X
- 하지만 오버헤드가 적고 지연이 거의 없어 빠름
- 메시지(Message) 기반 : 송신한 단위 그대로 전송
- 대표 사용 예시
    - 실시간 서비스 (VoIP, 스트리밍, 온라인 게임, DNS 조회 등)
    - 빠른 응답이 중요한 환경에서 사용

### TCP vs UDP 비교

| 구분 | TCP | UDP |
| --- | --- | --- |
| 연결 방식 | 연결형 (3-way handshake) | 비연결형 |
| 신뢰성 | 신뢰성 보장 (재전송, 순서 유지) | 비신뢰성 (손실 허용) |
| 전송 단위 | 바이트 스트림 | 데이터그램 (메시지 단위) |
| 속도 | 느림 (제어 오버헤드 많음) | 빠름 (제어 없음) |
| 혼잡 제어 | 있음 | 없음 |
| 흐름 제어 | 있음 | 없음 |
| 순서 보장 | 보장 | 미보장 |
| 헤더 크기 | 20바이트 이상 | 8바이트 |
| 대표 사용 예 | HTTP, FTP, SMTP, Telnet | DNS, VoIP, 스트리밍, 게임 |

## 관련 면접 질문

- TCP와 UDP의 가장 큰 차이점은 무엇인가요?
    - TCP는 연결형·신뢰성 보장·순서 보장, UDP는 비연결형·비신뢰성·빠른 전송에 초점.
- TCP의 3-Way Handshake 과정은 왜 필요한가요?
    - 송수신 측 모두 데이터 전송 준비가 되었음을 확인하고, 초기 시퀀스 번호를 동기화하기 위함입니다.
- UDP는 왜 신뢰성이 없는데도 많이 사용되나요?
    
    실시간성이 중요한 서비스에서는 손실보다 지연이 더 치명적이기 때문입니다.
    
    (예: 게임, 스트리밍, 화상통화 등)