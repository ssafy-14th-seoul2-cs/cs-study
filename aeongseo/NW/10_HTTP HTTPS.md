# HTTP / HTTPS

## HTTP(HyperText Transfer Protocol)

**1. 개념**
    - Application Layer에서 동작하는 비연결성 프로토콜
    - 크라이언트-서버 모델에 기반해 요청(request)과 응답(request)로 통신하는 구조
**2. 특징**
    - 비연결성(Connectionless) : 요청과 응답 끝나면 TCP 연결 종료. 각 요청은 독립적으로 처리
    - 무상태성(Stateless) : 서버는 클라이언트 상태 기억 X. 이전 요청 정보가 다음 요청에 영향 X
    - Request/Response 구조 : 클라이언트가 요청 보내면 서버가 응답 반환
    - 텍스트 기반 프로토콜 : 메시지는 사람이 읽을 수 있는 평문(ASCII) 형식으로 구성

**3. HTTP 메시지 구조**

```python
### Request Message
<시작줄>        : Method, Request-URI, HTTP-Version
<헤더(Header)>  : Host, User-Agent, Content-Type 등 메타데이터
<본문(Body)>    : 전송할 데이터 (POST, PUT 요청 등에서 사용)

### Response Message
<상태줄(Status Line)> : HTTP-Version, Status Code, Reason Phrase
<헤더(Header)>        : Content-Type, Server 등
<본문(Body)>          : 실제 콘텐츠 (HTML, JSON 등)
```

**4. 주요 HTTP 메서드**

    | 메서드 | 의미 | 특징 |
    | --- | --- | --- |
    | GET | 리소스 조회 | URL에 파라미터 포함, 캐싱 가능 |
    | POST | 리소스 생성 | 요청 본문에 데이터 포함 |
    | PUT  | 리소스 전체 수정 | 동일 URL에 여러 번 호출 시 결과 동일 |
    | PATCH | 리소스 일부 수정 | 비멱등성 |
    | DELETE | 리소스 삭제 | 멱등성 |
    | HEAD | 응답 본문 없이 헤더만 요청 | 상태 확인용 |
    - 멱등성(Idempotent) : 같은 요청 여러 번 보내도 결과가 변하지 않는 성질

**5. HTTP 한계**
    - 보안 취약점
        - 데이터가 평문으로 전송되어 도청, 위조, 변조 가능
        - 서버 신원 검증 기능 없음
    - 성능 문제
        - 요청당 하나의 TCP 연결
        - 헤더 중복 전송
        - 다중 요청 불가능

## HTTPS (HTTP Secure)

**1. 개념**
    - HTTP + SSL/TLS 계층 결합한 프로토콜
    - HTTP 메시지를 암호화된 통신 채널을 통해 전송하여 보안을 강화한 형태

**2. 전송 계층 구조 **

```python
Application Layer : HTTP
Security Layer    : SSL/TLS
Transport Layer   : TCP
```

**3. HTTPS의 보안 목표**
    - 기밀성(Confidentiality) : 데이터 암호화되어 도청해도 내용 파악 불가
    - 무결성(Integrity) : 데이터 전송 중 변조되지 않았음 보장
    - 인증(Authentication) : 서버의 신원 검증

**4. SSL/TLS 동작 원리**
    - HTTPS 는 대칭키 + 비대칭키 암호화 결합하여 사용
    - client hello
        - 클라이언트가 사용할 수 있는 암호화 알고리즘 목록, TLS 버전, 랜덤 값 등을 서버에 전송
    - server hello
        - 서버가 사용할 암호화 알고리즘 선택
        - 서버 인증서(SSL Certificate) 전달
    - 인증서 검증
        - 클라이언트는 CA의 공개키로 서버 인증서 검증 (신뢰할 수 있는 서버인지 확인)
    - 대칭키 교환 (세션키 생성)
        - 클라이언트는 서버의 공개키로 세션키 암호화하여 전달
        - 이후 통신은 빠른 대칭키 암호화로 진행

**5. 암호화 방식**

    | 암호 방식 | 설명 | 사용 시점 |
    | --- | --- | --- |
    | 비대칭키 암호화 (RSA, ECDHE) | 공개키로 암호화, 개인키로 복호화 | 세션키 교환 시 |
    | 대칭키 암호화 (AES, ChaCha20) | 동일 키로 암복호화 | 실제 데이터 전송 시 |
    | 해시 및 MAC (SHA, HMAC) | 데이터 무결성 검증 | 모든 통신 단계에서 |

**6. 인증서 (SSL Certificate)**
    - 서버의 신원을 증명하기 위해 공인인증기관(CA)이 발급한 디지털 서명 파일
    - 서버 도메인, 공개키, 유효기간, 발급자 서명 정보 포함
    - 인증서 검증 과정
        - 클라이언트는 인증서의 서명을 CA의 공개키로 검증
        - 브라우저는 신뢰된 루트 CA 목록 내장
        - 검증 실패 시 경고 표시