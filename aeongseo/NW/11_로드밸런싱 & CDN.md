## 로드밸런싱 & CDN

### 로드 밸런싱 (Load Balancing)

1. **정의**
    - 다중 서버 환경에서 트래픽 부하를 분산하는 네트워크 제어 기법
    - 목표 : 처리 효율 최적화(throughput), 지연 시간 최소화(latency), 장애 허용(fault tolerance)
    - 하나의 논리적 서비스를 여러 서버가 물리적으로 나누어 처리하면서 서비스 단일성(logical unity) 유지하는 구조
2. **계층별 로드 밸런싱**
    - L4 로드 밸런싱 (네트워크 계층 기반)
        - OSI 3계층 or 4계층에서 작동
        - 로드 밸런서는 IP 주소와 포트 번호 기반으로 세션 분배
        - 일반적으로 NAT or DSR 방식 사용
    - L7 로드 밸런싱 (애플리케이션 계층 기반)
        - OSI 7계층에서 작동 → HTTP, HTTPS, gRPC 등 애플리케이션 프로토콜의 내용 분석
        - 요청의 URL, 헤더, 쿠키, 세션 정보 기반으로 라우팅 결정
        - 콘텐츠 기반 트래픽 분산 가능
3. **핵심 동작 구조**
    1. 클라이언트 요청 수신 : DNS or IP 라우팅 통해 로드 밸런서로 트래픽 유입
    2. 서버 선택 알고리즘 적용 : 내부 정책 따라 적적한 백엔드 서버 선택 → 주로 스케줄링 알고리즘
    3. 세션 유지 : 클라이언트가 이전에 연결했던 서버로 다시 연결되도록 설정 가능
    4. 상태 모니터링 : 로드 밸런서는 백엔드 서버의 가용 상태를 주기적으로 점검 → 장애 서버는 자동으로 트래픽 분재 대상에서 제외
4. **주요 알고리즘**
    - Round Robin : 요청을 순차적으로 서버에 분배 (균등하지만 서버 성능 고려 X)
    - Weighted Round Robin : 서버별 처리 능력에 비례하여 요청 분배
    - Least Connection : 현재 연결 수가 가장 적은 서버 선택
    - IP Hash : 동일 클라이언트가 항상 같은 서버에 연결되도록 IP 기반 해시 사용
    - Dynamic Feedback : 각 서버의 응답 시간, CPU 부하 등을 실시간 측정하여 분배
5. **이론적 배경**
    - 로드 밸런싱은 분산 시스템의 리소스 할당 문제
    - 여러 자원이 병렬적으로 존재할 때 어떻게 요청을 효율적으로 분배하느냐가 핵십
    - 수학적 : 큐잉 이론(Queuing Theory)과 Markov Process 기반으로 성능 모델링, 지여시간, 처리율, 서버 활용도 간 트레이드오프 관계 분석
6. **기술적 구현 방식**
    - 하드웨어 기반 : F5 BIG-IP, Cisco ACE
    - 소프트웨어 기반 : NGINX, HAProxy, Envoy
    - 클라우드 서비스 : AWS ELB(ALB/NLB), GCP Load Balancer, Azure Load Balancer

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
        - 사용자가 `example.com/image.png` 요청
        - DNS 질의 시, CDN의 DNS 시스템이 가장 가까운 엣지 서버 IP 반환
        - 엣지 서버에 캐시된 콘텐츠 있으면 바로 응답
        - 없다면 Origin 서버에서 콘텐츠 가져와 저장(Cache Fill), 이후 동일 요청 시 바로 응답
3. **핵심 기술 요소**
    - Caching : 콘텐츠를 엣지 서버에 일정 기간 저장(TTL, Cache-Control 헤더 이용)
    - Geo-DNS / Anycast Routing : 사용자와 가까운 서버로 트래픽 라우팅
    - HTTP/2, QUIC 프로토콜 : 전송 효율 향상 및 연결 유지 최적화
    - Prefetching / Pipelining : 사용자 요청을 예측하여 미리 캐싱
4. **이론적 배경**
    - 네트워크 총 지연시간(Latency)
    
    $$
    Total Delay = Propagation Delay + Transmission Delay + Queuing Delay + Processing Delay
    $$
    
    - CDN은 Propagation Delay 최소화 위해 데이터를 물리적으로 사용자 근처에 위치한 서버에 미리 저장
5. **CDN 캐싱 정책**
    - TTL(Time-To-Live) : 캐시 유효 시간
    - LRU(Least Recently Used), LFU(Least Frequently Used) 등의 캐시 교체 알고리즘 사용
    - Cache Invalidation : 원본 데이터 변경 시 캐시 무효화 수행 (Consistency 관리)
6. **CDN의 보안적 측면**
    - DDoS 완화 : 엣지 서버들이 공격 트래픽 분산시켜 원본 서버 보호
    - TLS Termination : SSL 암호화를 엣지 서버에서 종료 → 암호화 오버헤드 분산
    - WAF 통합 : 웹 방화벽 기능으로 공격 탐지 및 차단