## REST API

### REST(Representational State Transfer)

1. **정의**

- 웹 서비스가 어떻게 동작해야 하는지에 대한 아키텍처 스타일 또는 설계 원칙
- 클라이언트와 서버 간의 상호작용 규정, 여러 가지 원칙과 제약 조건 갖고 있음

2. **REST 구성 요소**

- 자원(Resource) : 시스템에서 식별 가능한 모든 개체. URI로 식별
    - URI(Uniform Resource Identifier) : 통합 자원 식별자
- 자원에 대한 행위(HTTP Method) : 자원에 대한 조작 방법
- 자원에 대한 행위의 내용(Representations) : 자원의 현재 상태를 표현하는 데이터

3. **REST의 6가지 제약조건**

- Client-Server(서버-클라이언트 구조)
  - 클라이언트와 서버를 분리하여 독립적으로 발전 가능
  - 이로써 관심사의 분리(Separation of Concerns) 달성
    - 관심사의 분리 : 컴퓨터 프로그램을 구별된 부분으로 분리시키는 디자인 원칙
- Stateless(무상태)
  - 각 요청은 완전, 서버는 이전 요청의 상태 저장 X
  - 모든 요청은 독립적, 필요한 정보는 요청에 포함되어야 함
- Cacheable(캐시 처리 가능)
  - 응답은 명시적으로 캐시 가능해야 함
  - HTTP의 `Cache-Control`, `ETag` 등 활용
  - 효율성과 성능 향상에 기여
- Uniform Interface(인터페이스 일관성)
  - REST의 핵심 제약 조건
  - 시스템 구성요소 간 인터페이스를 일관되고 표준화 → 복잡성 감소
- Layered System(계층화)
  - 계층화된 시스템 아키텍처 사용
  - 각 계층은 독립적으로 동작 가능 → 보안성, 확장성 향상
- Code-on-Demand (optional)
  - 서버가 클라이언트에게 실행 가능한 코드 전송 가능 ex. HTML의 JavaScript
  - 선택적 제약조건, 동적인 확장성 허용 → 보안상 추천 X

4. **REST 장점 & 한계**

- 장점
  - 확장성 (Scalability) : 무상태성 덕분에 서버 간 부하 분산 쉬움
  - 가시성 (Visibility) : 요청이 자명하게 표현되어 모니터링 용이
  - 수정 용이성 (Modifiability) : 클라이언트와 서버가 독립적으로 발전 가능
  - 성능 (Performance) : 캐시와 계층 구조를 통한 효율적 처리 가능
- 한계
  - 상태 유지의 어려움 : 로그인 세션 등 지속적 상태 관리 어려움
  - 표현력 한계 : 복잡한 관계형 데이터 표현에는 비효율적
  - Over-fetching / Under-fetching : 필요한 데이터보다 많거나 적은 데이터 가져올 수 있음

---

### REST API

1. **정의**

- REST 기반으로 서비스 API를 구현한 것

2. **REST API 설계 규칙**

- URI 작성 시 소문자 사용
- 언더바 대신 하이픈 사용
- 마지막에 슬래시 포함 X
- URI에 행위 포함 X
- 파일 확장자는 URI에 포함 X
- 전달하고자 하는 명사 사용, 컨트롤 자원을 의미하는 경우 예외적으로 동사 허용
- URI에 작성되는 영어는 복수형으로 작성

---

### RESTful

1. **정의**

- REST 원리를 따르는 시스템을 RESTful 하다고 함
- REST API를 사용하여도 RESTful하지 않을 수 있음

2. **목적**

- 이해하기 쉽고 사용하기 쉬운 REST API 만듦
- 일관적인 컨벤션을 통한 API의 이해도 및 호환성 향상