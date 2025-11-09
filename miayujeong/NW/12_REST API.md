## 📖 REST (Representational State Transfer)

: 웹에서 자원의 상태(Representation)를 전송(Transfer)하는 아키텍처 스타일

- 자원을 이름으로 구분하고, 그 자원에 대한 행위(HTTP Method)를 정의해 데이터를 주고받는 구조
- HTTP를 기반으로 하므로, 별도의 전송 프로토콜 없이 웹 표준만으로 API 설계 가능

### REST 구성 요소

| 구성요소 | 설명 | 예시 |
| --- | --- | --- |
| **자원(Resource)** | 서버가 제공하는 데이터 또는 기능 | `/users`, `/posts`, `/products` |
| **행위(Verb)** | 자원에 수행할 동작 (HTTP Method) | `GET`, `POST`, `PUT`, `DELETE`, `PATCH` |
| **표현(Representation)** | 자원의 상태를 표현하는 데이터 포맷 | JSON, XML, HTML, RSS 등 |

#### URI, URL, URN

|  | 설명 | 예시 |
| --- | --- | --- |
| **URI (Uniform Resource Identifier)** | 자원을 식별하기 위한 **고유 식별자** (URL, URN 포함 개념) | `https://api.example.com/users/1` |
| **URL (Uniform Resource Locator)** | 자원의 **위치**를 나타냄 (URI의 하위 개념) | `https://example.com/products/10` |
| **URN (Uniform Resource Name)** | 자원의 **이름**을 나타냄 (위치와 무관) | `urn:isbn:9781234567890` |

#### CRUD operation & HTTP Method

| 동작 | HTTP Method | 예시 | 설명 |
| --- | --- | --- | --- |
| Create | `POST` | `/users` | 새로운 리소스 생성 |
| Read | `GET` | `/users/1` | 특정 리소스 조회 |
| Update | `PUT` / `PATCH` | `/users/1` | 리소스 수정 |
| Delete | `DELETE` | `/users/1` | 리소스 삭제 |

<br>

### REST 특징

- **Client–Server 구조**
    - 클라이언트는 UI/UX 담당, 서버는 데이터 관리 담당
    - 서로의 역할이 분리되어 유지보수가 용이
- **Stateless (무상태성)**
    - 서버는 클라이언트의 상태를 저장하지 않음
    - 각 요청은 독립적으로 처리 (이전 요청의 정보에 의존하지 않음)
- **Cacheable (캐시 처리 가능)**
    - 응답에 캐시 가능 여부를 명시 → 성능 향상, 네트워크 부하 감소
- **Layered System (계층 구조)**
    - 클라이언트는 중간 서버(프록시, 로드밸런서, 게이트웨이 등) 존재를 인식하지 않아도 됨
- **Uniform Interface (일관된 인터페이스)**
    - URI 설계, 메서드 의미, 응답 구조가 일관되어야 함
- **Code on Demand (선택 사항)**
    - 서버가 클라이언트에 코드를 전송해 실행할 수 있음 (예: JS 코드)

<br>

### REST 필요성

- 모바일, 웹, IoT 등 **다양한 클라이언트 환경**에서도 통일된 통신 방식을 사용하기 위함
- 시스템 간 **유연한 확장성**과 **독립성** 확보
- HTTP 기반이라 별도의 라이브러리나 프로토콜 없이 **전 세계 어디서든 접근 가능**

<br>

### REST의 장단점

#### 장점

- HTTP 표준 기반이라 별도의 인프라 불필요
- 플랫폼 독립적 (언어, OS 제약 없음)
- 클라이언트-서버 구조로 역할 명확
- 확장성, 재사용성, 유지보수 용이

#### 단점

- 명확한 표준이 없어 설계자마다 차이 발생
- HTTP 메서드만으로 복잡한 트랜잭션 표현 어려움
- 브라우저 테스트 시 Header 기반 요청 처리의 불편함
- 구형 환경에서는 일부 메서드 미지원

<br>

## 📖 REST API

: REST의 원리를 따르는 API

### REST API 설계

#### REST API 설계 기본 예시 

| 행위 | HTTP Method | URI 예시 | 설명 |
| --- | --- | --- | --- |
| 사용자 전체 조회 | GET | `/users` | 사용자 목록 조회 |
| 사용자 추가 | POST | `/users` | 사용자 생성 |
| 특정 사용자 조회 | GET | `/users/{id}` | 사용자 정보 조회 |
| 특정 사용자 수정 | PUT | `/users/{id}` | 사용자 정보 갱신 |
| 특정 사용자 삭제 | DELETE | `/users/{id}` | 사용자 삭제 |

#### 올바른 REST API 설계 규칙 

- URI는 명사 중심 (동사 X)
    
    ```
    /getUser ❌
    /users   ✅
    ```
    
- 소문자 사용, 언더바(_) 대신 하이픈(-)
    
    ```
    /user_name ❌
    /user-name ✅
    ```
    
- 마지막에 슬래시(/) 붙이지 않기
    
    ```
    /myname/  ❌
    /myname   ✅
    ```
    
- 파일 확장자 포함하지 않기
    
    ```
    /photo.jpg  ❌
    /photo      ✅
    ```
    
- 리소스 간 계층 구조 표현
    
    ```
    /users/1/orders/5  -- "1번 사용자의 5번 주문"
    ```
    
<br>

### RESTful

- REST의 원칙을 충실히 지킨 시스템
- 단순히 HTTP를 쓴다고 RESTful한 것은 아님
    
    
    | RESTful하지 않은 설계 | RESTful 설계 |
    | --- | --- |
    | `POST /getUserInfo` | `GET /users/{id}` |
    | `POST /updateUser` | `PUT /users/{id}` |
    | `/userDelete.do` | `DELETE /users/{id}` |

#### RESTful의 목적

- 누구나 쉽게 이해하고 사용할 수 있는 API
- 일관된 컨벤션을 통해 **유지보수성과 확장성 향상**
- 단, 성능·보안 등 특수한 상황에서는 굳이 완전한 RESTful 구조를 강제할 필요 없음

#### REST vs. RESTful

|  | REST | RESTful |
| --- | --- | --- |
| 개념 | 아키텍처 원칙 (설계 철학) | REST 원칙을 따르는 구현 |
| 중심 | 리소스 중심 설계 | 리소스 + 일관된 규칙 |
| 조건 | HTTP 사용 | REST의 6원칙 충실히 따름 |
