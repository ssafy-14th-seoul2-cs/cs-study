## 로드밸런싱 & CDN

### 로드 밸런싱 (Load Balancing)

1. **정의**

- 다중 서버 환경에서 트래픽 부하를 분산하는 네트워크 제어 기법
- 목표 : 처리 효율 최적화(throughput), 지연 시간 최소화(latency), 장애 허용(fault tolerance)
- 하나의 논리적 서비스를 여러 서버가 물리적으로 나누어 처리하면서 서비스 단일성(logical unity) 유지하는 구조

2. **계층별 로드 밸런싱**

- L4 로드 밸런싱 (네트워크 계층 기반)
  - IP 주소, 포트 번호, MAC 주소, 전송 프로토콜 등에 따라 트래픽 분산
  - 속도 빠름, 효율 높음, 복호화 필요 없어 안전, 가격 저렴
  - 섬세한 라우팅 불가, 사용자 IP가 수시로 바뀌면 연속적인 서비스 제공 어려움
- L7 로드 밸런싱 (애플리케이션 계층 기반)
  - 요청의 URL, 헤더, 쿠키, 세션 정보 기반으로 라우팅 결정
  - L4 로드밸런서 기능 + 패킷 내용 확인, 내용에 따라 로드를 특정 서버에 분배
  - Dos/DDos 같은 비정상적인 트래픽 필터링 가능
  - 섬세한 라우팅, 캐싱 기능, 서비스 안정성 높음
  - 비쌈, 패킷 내용 복호화 → 더 높은 비용, 클라이언트가 로드밸런서와 인증서 공유 → 보안상의 위험성

3. **핵심 동작 구조**

- 클라이언트 요청 수신 : DNS or IP 라우팅 통해 로드 밸런서로 트래픽 유입
- 서버 선택 알고리즘 적용 : 내부 정책 따라 적적한 백엔드 서버 선택 → 주로 스케줄링 알고리즘
- 세션 유지(Session Persistence) : 클라이언트가 이전에 연결했던 서버로 다시 연결되도록 설정 가능
- 상태 모니터링(Health Check) : 로드 밸런서는 백엔드 서버의 가용 상태를 주기적으로 점검 → 장애 서버는 자동으로 트래픽 분배 대상에서 제외

4. **주요 알고리즘**

- Round Robin : 요청을 순차적으로 서버에 분배 (균등하지만 서버 성능 고려 X)
- Weighted Round Robin : 서버별 처리 능력에 비례하여 요청 분배
- Least Connection : 현재 연결 수가 가장 적은 서버 선택
- IP Hash : 동일 클라이언트가 항상 같은 서버에 연결되도록 IP 기반 해시 사용
- Least Response Time Method : 서버의 현재 연결 상태와 응답시간 모두 고려, 가장 짧은 응답시간을 보내는 서버로 트래픽 할당

---

### CDN (Content Delivery Network)

1. **정의**

- 지리적으로 분산된 캐시 서버(Edge Server) 네트워크 통해 사용자에게 가장 가까운 노드에서 콘텐츠 제공하는 분산 전송 시스템
- 목표 : 지연 시간 감소(Latency Reduction), 트래픽 부하 완화(Offloading)

2. **작동 원리**

- 기본 구조
  - Origin Server : 원본 콘텐츠가 저장된 중앙 서버
  - Edge Server : 전 세계에 분산된 캐시 서버, 사용자 요청에 빠르게 응답
  - CDN Routing System : 사용자의 위치, 네트워크 상태, 서버 부하 등 고려하여 가장 적합한 엣지 서버 선택 (주로 DNS 기반 라우팅)
- 동작 절차
  - 사용자가 콘텐츠 요청
  - DNS 질의 시, CDN의 DNS 시스템이 가장 가까운 엣지 서버 IP 반환
  - 엣지 서버에 캐시된 콘텐츠 있으면 바로 응답
  - 없다면 Origin 서버에서 콘텐츠 가져와 저장(Cache Fill), 이후 동일 요청 시 바로 응답

3. **Caching 방식**

- Static Caching
    - Origin Server에 있는 Content를 운영자가 미리 Cache Server에 복사, 사용자가 Cache Server에 Content 요청 시 Cache Server에 존재
    - 대부분의 국내 CDN에서 사용 (게임 클라이언트 다운로드 등)
- Dynamic Caching
    - Origin Server에 있는 Content를 운영자가 미리 Cache Server에 복사 X
    - 사용자가 Content 요청 시 해당 Content가 없는 경우 Origin Server로부터 다운로드 받아 전달. 있는 경우에는 캐싱된 Content 전달
    - 각각의 Content는 일정 시간 이후 Cache Server에서 삭제