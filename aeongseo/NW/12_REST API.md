## REST API

### REST

1. **정의**
    - 2000년 로이 필딩이 박사 논문에 제시한 웹 아키텍처의 아키텍처 스타일(architectural style)
    - 분산 하이퍼미디어 시스템(ex. WWW)을 설계하기 위한 원리와 제약조건의 집합
2. **철학적 기반**
    - 확장성과 단순성, 독립성 극대화하기 위한 설계 원칙 추출
3. **REST의 6가지 제약조건 (Architectural Constraints)**
    - 제약 조건을 모두 만족할수록 더 RESTful하다고 평가
    1. Client-Server
        - 클라이언트와 서버를 분리하여 독립적으로 발전 가능
        - 이로써 관심사의 분리(Separation of Concerns) 달성
    2. Stateless
        - 각 요청은 완전, 서버는 이전 요청의 상태 저장 X
        - 모든 요청은 독립적, 필요한 정보는 요청에 포함되어야 함
    3. Cacheable
        - 응답은 명시적으로 캐시 가능해야 함
        - HTTP의 `Cache-Control`, `ETag` 등 활용
        - 효율성과 성능 향상에 기여
    4. Uniform Interface
        - REST의 핵심 제약 조건
        - 시스템 구성요소 간 인터페이스를 일관되고 표준화 → 복잡성 감소
    5. Layered System
        - 클라이언트는 중간 계층 인식 X
        - 각 계층은 독립적으로 동작 가능 → 보안성, 확장성 향상
    6. Code-on-Demand (optional)
        - 서버가 클라이언트에게 실행 가능한 코드 전송 가능
        - 선택적 제약조건, 동적인 확장성 허용
4. **Uniform Interface의 4가지 원칙**
    - Identification of Resources : 자원은 URI(Uniform Resource Identifier)로 식별
    - Manipulatino of Resources through Representations : 자원의 상태는 Representationn을 통해 전송, 조작. 클라이언트는 자원 자체가 아니라 자원의 표현 다룸
    - Self-descriptive Messages : 각 메시지는 독립적으로 이해 가능해야 함
    - Hypermedia as the Engine of Application State(HATEOAS) : 클라이언트는 하이퍼링크 통해 상태 전이 수행해야 함. 응답 메시지 않에 다음 가능한 액션의 링크 제공해야 함
5. **REST의 구성요소 모델  (Components)**
    - 자원 (Resource) : 시스템에서 식별 가능한 모든 개체. URI로 식별됨
    - 표현 (Representation) : 자원의 현재 상태를 표현하는 데이터
    - 행위 (HTTP Method) : 자원에 대한 조작 방법
6. **REST의 전송 프로토콜**
    - REST는 HTTP 기반으로 하지만, HTTP에 종속적 X
    - REST는 개념적 모델, HTTP 이외의 프로토콜에서도 구현 가능
    - 실제 웹 환경에서는 HTTP 요청/응답 구조가 REST 원칙과 자연스럽게 일치
7. **REST의 설계 철학**
    - Statelessness (무상태성)
        - 요청 간의 의존성 최소화해 확장성(salability)과 단순성(simplicity) 얻음
    - Uniform Interface (일관성)
        - 모든 리소스가 동일 방식으로 표현, 접근 가능해야 함
        - 클라이언트는 서버의 내부 구조를 몰라도 자원 조작 가능
8. **REST 장점 & 한계**
    - 장점
        - 확장성 (Scalability) : 무상태성 덕분에 서버 간 부하 분산 쉬움
        - 가시성 (Visibility) : 요청이 자명하게 표현되어 모니터링 용이
        - 수정 용이성 (Modifiability) : 클라이언트와 서버가 독립적으로 발전 가능
        - 성능 (Performance) : 캐시와 계층 구조를 통한 효율적 처리 가능
    - 한계
        - 상태 유지의 어려움 : 로그인 세션 등 지속적 상태 관리 어려움
        - 표현력 한계 : 복잡한 관계형 데이터 표현에는 비효율적
        - Over-fetching / Under-fetching : 필요한 데이터보다 많거나 적은 데이터 가져올 수 있음
9. **REST API vs RESTful API**
    - REST API : 개념을 이용한 모든 API
    - RESTful API : REST의 제약조건을 충실히 준수한 API
    → 모든 REST API가 RESTful X