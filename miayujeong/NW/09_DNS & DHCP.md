## 📖 DNS (Domain Name System)

> 사람이 읽는 도메인 이름 (ex. www,naver.com)을 컴퓨터가 사용하는 IP 주소 (ex. 223.130.195.95)로 변환하는 시스템
> 
- 사람은 **이름(name)** 으로, 라우터는 **주소(address)** 로 통신
- IP 주소를 기억하지 않아도 웹사이트 접속 가능
- DNS는 **분산 데이터베이스 + 애플리케이션 계층 프로토콜**로 동작
- UDP 53번 포트 사용
- 클라이언트–서버 구조 (브라우저 → DNS 서버)

<br>

### DNS 주요 기능

|  |  |
| --- | --- |
| **Hostname → IP 변환** | 웹 브라우저, 메일 등에서 도메인을 IP로 변환 |
| **호스트/메일 서버 별칭(alias)** | 별칭(host alias)에 대한 실제 이름(canonical name) 조회 |
| **부하 분산 (Load Distribution)** | 여러 서버가 한 도메인을 공유할 경우, 요청을 분산 |
| **캐싱 (Caching)** | 자주 사용하는 주소 매핑 정보를 로컬에 저장해 성능 향상 |

<br>

### DNS 계층 구조

```
Root (.)
 ├── .com
 │    ├── google.com
 │    └── amazon.com
 └── .kr
      ├── naver.kr
      └── daum.kr
```

- **Root DNS Servers**
    - 전 세계에 분산된 13개 루트 서버(복제 포함 수백 대)
    - `.com`, `.org`, `.kr` 등의 **TLD 서버 주소 제공**
    - ICANN이 관리
- **TLD (Top Level Domain) Servers**
    - `.com`, `.net`, `.edu`, `.kr` 등 상위 도메인 관리
    - 각 도메인에 해당하는 **authoritative 서버의 IP 주소 제공**
- **Authoritative DNS Servers**
    - 조직(기업, 학교 등)의 자체 DNS 서버
    - 실제 도메인 이름과 IP 주소의 매핑 데이터 보유
- **Local DNS Server**
    - 사용자의 ISP 또는 사내 네트워크에 위치
    - ISP나 기업, 학교 단위에서 운영
    - 클라이언트의 DNS 요청을 대신 처리하고 캐싱 수행

<br>

### DNS 동작 과정

<img src="images/NW_03_4.png">

1. 사용자가 브라우저에 URL 입력
2. 컴퓨터는 **로컬 DNS 캐시**에 해당 주소가 있는지 확인
3. 없으면 **로컬 DNS 서버(Resolver)** 에 요청
4. Resolver는 루트 → TLD → 권한 DNS 서버 순으로 조회
5. IP 주소를 찾아 사용자에게 반환
6. 브라우저는 해당 IP로 TCP 연결 시작

<br>

### 특징 요약

- **캐싱(Cache)** : 동일 질의 반복 시 응답 속도 향상
- **분산 구조** : 중앙 서버 장애 시에도 전체 시스템 유지
- **UDP 53번 포트** 사용 (쿼리 빠름), 일부는 TCP 사용
- DHCP 서버가 클라이언트에 DNS 서버 주소를 함께 제공함

<br>

## 📖 DHCP (Dynamic Host Configuration Protocol)

> 네트워크에 연결된 장치에 IP 주소, 서브넷 마스크, 게이트웨이, DNS 주소 등을 자동으로 할당하는 프로토콜
> 
- 장치가 네트워크에 접속할 때마다 자동으로 설정값을 받아오게 함
- 수동 설정 필요 x → IP 충돌 방지, 관리 효율 향상
- 임대 개념 (IP를 일정 기간 동안만 부여)

<br>

### DHCP 주요 구성 요소

| 구성 요소 | 역할 |
| --- | --- |
| **DHCP 서버** | IP 주소 풀 관리 및 임대 (보통 라우터 내부에 존재) |
| **DHCP 클라이언트** | 네트워크 연결 시 IP 주소 요청 (PC, 스마트폰 등) |
| **DHCP 릴레이 에이전트** | 다른 서브넷의 DHCP 요청을 중계 (대형 네트워크용) |

<br>

### DHCP 임대 절차

> DORA (Discovery, Offer, Request, Ack)
> 

|  | 메시지 | 방향 | 설명 |
| --- | --- | --- | --- |
| ① | **Discover** | Client → Broadcast | DHCP 서버를 찾기 위한 요청 |
| ② | **Offer** | Server → Client | 사용 가능한 IP 주소 제안 |
| ③ | **Request** | Client → Server | 제안받은 IP 중 하나 선택 및 요청 |
| ④ | **Acknowledge (ACK)** | Server → Client | IP 주소 임대 승인 및 설정 완료 |

<br>

### 특징 요약

- **자동 설정** : IP, DNS, Gateway 모두 자동 배정
- **임대 기반 관리** : 유휴 IP 낭비 방지
- **중앙 집중 관리** : 서버에서 일괄 관리 가능
- **충돌 방지** : 동일 IP 중복 방지

<br>

## 💭 DNS, DHCP

|  | DNS | DHCP |
| --- | --- | --- |
| **기능** | 도메인 이름 ↔ IP 주소 변환 | 네트워크 장치에 IP 주소 자동 할당 |
| **역할** | "이름 해석(Name Resolution)" | "주소 배포(Address Assignment)" |
| **작동 시점** | 사용자가 웹 접근 시 | 장치가 네트워크에 처음 연결될 때 |
| **프로토콜** | UDP/TCP 53 | UDP 67(서버), 68(클라이언트) |
| **결합 방식** | DHCP가 DNS 서버 주소를 함께 제공 | DNS는 DHCP가 부여한 IP를 사용해 이름 해석 |