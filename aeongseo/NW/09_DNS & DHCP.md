# DNS & DHCP

## DNS (Domain Name System)

**1. 개념**

- 도메인 이름을 IP 주소로 바꿔주는 시스템

**2. 필요성**

- 사람은 도메인을 기억, 컴퓨터는 숫자로 이해
- DNS가 인터넷의 전화번호부 역할

**3. 동작 방식**

1. 사용자가 [`www.naver.com`](http://www.naver.com) 입력
2. 로컬 DNS 서버에 IP 주소 요청
3. 캐시에 없으면 루트 DNS 서버로 문의
4. 루트 서버가 .com 서버로 요청
5. .com 서버에서 찾음
6. 최종적으로 naver.com의 IP 주소 받아옴
7. IP 주소를 브라우저에 전달해서 접속 완료
- ⇒ 재귀적 질의(Recursive Query)
    
**4. 주요 구성 요소**

- DNS 클라이언트 : 질의하는 쪽 (PC or 스마트폰)
- DNS 서버 : IP 정보를 제공하는 쪽
- 루트 DNS 서버 : DNS 계층의 최상위, 전 세계에 13개 존재
- TLD 서버 : `.com`, `.kr`, `.org` 등 도메인 구분
- 권한 있는 네임서버(Authoritative NS) : 실제 도메인 정보(IP 주소)를 가지고 있음

## DHCP (Dynamic Host Configuration Protocol)

**1. 개념**

- IP 주소를 자동으로 할당해주는 프로토콜
- 네트워크 관리 자동화 위함

**2. 동작 방식 (DORA 과정)**

1. Discover : 클라이언트가 IP 요청 브로드캐스트 날림
2. Offer : DHCP 서버가 IP 제안
3. Request : 클라이언트가 사용 요청
4. Acknowledge : 서버가 승인 응답
- 클라이언트는 IP, 서브넷 마스크, 게이트웨이, DNS 서버 주소 등을 자동으로 받음

**3. 장점**

- 자동화 : 관리자가 일일이 IP 설정 X
- 중복 방지 : 같은 IP가 두 번 할당 X
- 효율성 : 임시로 접속하는 장치에 유용

**4. DHCP 서버와 클라이언트**

- 서버 : IP 주소 풀(pool)을 가지고 관리
- 클라이언트 : IP를 자동으로 받는 장치
- lease : IP는 영구 소유가 아니라 일정 시간 동안 임대 개념, 만료되면 갱신 요청

## DNS와 DHCP 관계

- DHCP는 IP 주소 자동 할당
- DNS는 도메인 이름을 자동으로 해석
- Dynamic DNS (DDNS) : DHCP가 IP 주소 할당을 DNS에 알리면, DNS는 기록
