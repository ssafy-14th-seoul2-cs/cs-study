# REST API

- **REpresentational State Transfer**의 약자로, 웹에서 데이터를 주고받는 방식 중 하나
- 클라이언트(웹 브라우저, 앱 등)와 서버가 서로 통신할 때 지켜야 하는 일종의 약속
- ex. 앱에서 게시물을 보거나 좋아요를 누를 때, 앱과 서버가 REST API를 통해 데이터를 주고받는 것

## REST의 기본 개념

### 자원(Resource)
서버가 가지고 있는 데이터나 기능. 모든 자원은 고유한 **URI(주소)**로 식별된다.

- 사용자 정보: `/users`
- 특정 사용자: `/users/123`
- 게시물 목록: `/posts`
- 특정 게시물: `/posts/456`

### 표현(Representation)
자원을 어떤 형태로 표현할지를 말한다. 보통 **JSON** 형식을 많이 사용함

```json
{
  "id": 123,
  "name": "김철수",
  "email": "kim@example.com"
}
```

### 상태 전달(State Transfer)
클라이언트가 서버의 자원을 요청하면, 서버는 자원의 현재 상태를 전달해준다.

## HTTP 메서드 (동작 방식)

REST API는 HTTP 메서드를 사용해서 자원에 대한 작업을 표현한다.(**CRUD**)

### GET - 조회 (Read)
자원을 가져올 때 사용. 데이터를 읽기만 하고 변경하지 않는다.

```
GET /users          → 모든 사용자 목록 가져오기
GET /users/123      → ID가 123인 사용자 정보 가져오기
GET /posts?page=2   → 2페이지의 게시물 목록 가져오기
```

### POST - 생성 (Create)
새로운 자원을 만들 때 사용.

```
POST /users         → 새로운 사용자 생성
POST /posts         → 새로운 게시물 작성
```

요청할 때 body에 데이터를 담아서 보낸다:
```json
{
  "name": "이영희",
  "email": "lee@example.com"
}
```

### PUT / PATCH - 수정 (Update)
기존 자원을 수정할 때 사용.

- **PUT**: 자원 전체를 교체
- **PATCH**: 자원의 일부만 수정

```
PUT /users/123      → ID가 123인 사용자 정보 전체 수정
PATCH /users/123    → ID가 123인 사용자 정보 일부 수정
```

### DELETE - 삭제 (Delete)
자원을 삭제할 때 사용.

```
DELETE /users/123   → ID가 123인 사용자 삭제
DELETE /posts/456   → ID가 456인 게시물 삭제
```

## REST API 설계 원칙

### 1. URI는 명사를 사용
동작은 HTTP 메서드로 표현하고, URI는 자원을 나타내야 한다.

**좋은 예:**
```
GET /users          → 사용자 목록 조회
POST /users         → 사용자 생성
DELETE /users/123   → 사용자 삭제
```

**나쁜 예:**
```
GET /getUsers          → 동사 사용 (X)
POST /createUser       → 동사 사용 (X)
GET /deleteUser/123    → GET으로 삭제 (X)
```

### 2. 계층 구조 표현
URI는 슬래시(/)로 계층 관계를 나타낸다.

```
/users/123/posts           → 사용자 123의 게시물들
/users/123/posts/456       → 사용자 123의 게시물 456
/posts/456/comments        → 게시물 456의 댓글들
```

### 3. 소문자 사용
URI는 소문자를 사용하고, 단어 구분은 하이픈(-)을 사용한다.

```
/user-profiles     (O)
/user_profiles     (△)
/UserProfiles      (X)
```

### 4. 파일 확장자 포함하지 않기
```
/users/profile-image     (O)
/users/profile-image.jpg (X)
```

### 5. 무상태성 (Stateless)
서버는 클라이언트의 상태를 저장하지 않는다. 매 요청은 독립적.

- 로그인 정보 같은 건 **토큰**을 사용해서 매번 함께 보낸다
- 서버가 이전 요청을 기억할 필요가 없음

## REST API의 장점

**1. 단순하고 직관적**
- HTTP를 그대로 활용하니까 별도 인프라가 필요 없다
- URI만 봐도 어떤 자원인지 알 수 있다

**2. 플랫폼 독립적**
- 어떤 프로그래밍 언어로도 사용 가능
- 웹, 앱, IoT 기기 등 어디서든 사용할 수 있다

**3. 확장성**
- 서버와 클라이언트가 독립적이라 각각 개발하기 쉽다
- 무상태성 덕분에 서버를 쉽게 확장할 수 있다

**4. 캐싱 가능**
- GET 요청은 캐싱할 수 있어서 성능이 좋아진다
