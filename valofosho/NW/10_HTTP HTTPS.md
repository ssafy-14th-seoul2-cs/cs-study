# 🌐 HTTP & HTTPS 정리

> 웹에서 데이터를 주고받는 핵심 프로토콜  
> - **HTTP (HyperText Transfer Protocol)**  
> - **HTTPS (HyperText Transfer Protocol Secure)**

---

## 📘 1️⃣ HTTP (HyperText Transfer Protocol)

> **웹 브라우저와 서버 간 데이터 송수신을 위한 비연결성, 비암호화 프로토콜**

---

### ⚙️ 기본 개념

| 항목 | 내용 |
|:--|:--|
| **역할** | 클라이언트와 웹 서버 간 데이터 전송 |
| **계층** | 응용 계층 (TCP 기반) |
| **포트 번호** | 80 |
| **특징** | 비연결(Connectionless), 비상태(Stateless), 평문(Plaintext) 전송 |

---

### 📡 HTTP 동작 구조

```

[Client] ↔ [Server]
├─ 요청(Request): URL, 메서드, 헤더 등
└─ 응답(Response): 상태 코드, 데이터(HTML, JSON 등)

```

---

### 📤 HTTP 요청 메시지 구조

```

GET /index.html HTTP/1.1
Host: [www.example.com](http://www.example.com)
User-Agent: Chrome/123.0
Accept: text/html

```

---

### 📥 HTTP 응답 메시지 구조

```

HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1024

<html> ... </html>
```

---

### 🧩 주요 메서드

| 메서드         | 설명              |
| :---------- | :-------------- |
| **GET**     | 리소스 조회          |
| **POST**    | 데이터 생성 (폼 제출 등) |
| **PUT**     | 데이터 전체 수정       |
| **PATCH**   | 데이터 일부 수정       |
| **DELETE**  | 리소스 삭제          |
| **HEAD**    | 헤더 정보만 요청       |
| **OPTIONS** | 지원하는 메서드 조회     |

---

### ⚠️ HTTP의 한계

| 문제         | 설명                  |
| :--------- | :------------------ |
| **평문 통신**  | 데이터 암호화 없음 → 도청 가능  |
| **무결성 부재** | 중간에 데이터 변조 가능       |
| **인증 부재**  | 서버 신원 확인 불가 (피싱 위험) |

---

## 🔐 2️⃣ HTTPS (HTTP Secure)

> **HTTP + SSL/TLS 암호화 계층**
> HTTP 통신을 **TLS(Transport Layer Security)** 로 감싸
> **기밀성, 무결성, 인증**을 확보한 보안 프로토콜

---

### ⚙️ 기본 개념

| 항목          | 내용                                                  |
| :---------- | :-------------------------------------------------- |
| **역할**      | HTTP 통신에 암호화·인증·무결성을 추가                             |
| **계층**      | 응용 계층 (HTTP) + 보안 계층 (SSL/TLS)                      |
| **포트 번호**   | 443                                                 |
| **기반 프로토콜** | TCP                                                 |
| **보안 기능**   | 암호화(Encryption), 인증(Authentication), 무결성(Integrity) |

---

### 📊 HTTPS 구조

```
[Application Layer]  HTTP
[Security Layer]     TLS/SSL
[Transport Layer]    TCP
[Network Layer]      IP
```

---

### 🧠 HTTPS가 제공하는 3대 보안 기능

| 기능                        | 설명                        |
| :------------------------ | :------------------------ |
| **기밀성 (Confidentiality)** | 데이터 암호화를 통해 도청 방지         |
| **무결성 (Integrity)**       | 전송 중 데이터 변조 탐지 (MAC/Hash) |
| **인증 (Authentication)**   | 인증서를 통한 서버 신원 확인 (CA 기반)  |

---

### 📡 HTTPS 동작 절차 (TLS Handshake)

> 클라이언트와 서버가 **암호화 통신을 시작하기 전**,
> 서로 신뢰 관계를 형성하고 세션 키를 교환하는 과정

```
[Client]                                 [Server]
   │                                           │
①  │ ── ClientHello ─────────────────────────> │ (암호화 방식 제안)
②  │ <────────────────────── ServerHello ───── │ (방식 선택, 인증서 전달)
③  │ <────────────────── Certificate (공개키) ─ │ (서버 신원 확인)
④  │ ── ClientKeyExchange (세션키 생성 정보) → │
⑤  │ ── ChangeCipherSpec / Finished ─────────> │ (암호화 시작)
⑥  │ <────────────── ChangeCipherSpec / Finished │ (서버 응답)
```

> 이후 모든 HTTP 요청/응답은 **암호화된 세션키**로 안전하게 전송됨.

---

### 🧩 인증서 (SSL/TLS Certificate)

| 항목        | 설명                                       |
| :-------- | :--------------------------------------- |
| **발급 주체** | 인증 기관 (CA, Certificate Authority)        |
| **포함 정보** | 서버 도메인, 공개키, 서명, 유효기간                    |
| **역할**    | 서버 신원 보증 + 암호화 키 교환                      |
| **형식**    | PEM / DER (ex: `cert.pem`, `server.crt`) |

---

### 🧠 TLS 암호화 방식

| 암호화 단계      | 사용 알고리즘             | 설명                     |
| :---------- | :------------------ | :--------------------- |
| **키 교환**    | RSA, Diffie-Hellman | 대칭키 교환용 (초기 Handshake) |
| **데이터 암호화** | AES, ChaCha20       | 대칭키로 실제 데이터 암호화        |
| **무결성 검증**  | HMAC-SHA256         | 데이터 위·변조 검출            |

---

### 🔑 HTTPS 인증서 신뢰 체계

```
Root CA
 └── Intermediate CA
      └── Server Certificate
```

* 브라우저는 Root CA를 신뢰 기반으로 내장
* 서버 인증서는 CA의 서명(Signature)으로 신뢰 확보

---

### ⚙️ HTTP vs HTTPS 비교

| 구분          | HTTP        | HTTPS            |
| :---------- | :---------- | :--------------- |
| **보안성**     | 없음 (평문 전송)  | 높음 (암호화 통신)      |
| **포트 번호**   | 80          | 443              |
| **프로토콜 계층** | 응용 계층       | 응용 + TLS 계층      |
| **데이터 암호화** | X           | O (세션키 암호화)      |
| **인증서 필요**  | X           | O (CA 서명 필요)     |
| **속도**      | 빠름          | 약간 느림 (암호화 부하)   |
| **대표 사용처**  | 내부망, 테스트 환경 | 웹 서비스, 로그인, 결제 등 |

---

### 🧩 예시 흐름

#### ✅ HTTP

```
브라우저 → 서버
"GET /login.html"  →  (평문 데이터 전송)
```

#### 🔒 HTTPS

```
브라우저 → 서버
[TLS Handshake 후] → "GET /login.html" (암호화된 요청)
```

---

### 💡 추가 보안 개념

| 용어                                        | 설명                                        |
| :---------------------------------------- | :---------------------------------------- |
| **HSTS (HTTP Strict Transport Security)** | 브라우저가 HTTP 대신 HTTPS로만 접속하도록 강제            |
| **Let's Encrypt**                         | 무료 SSL 인증서 제공 기관 (CA)                     |
| **TLS 버전**                                | TLS 1.2, 1.3이 현재 표준 (SSL은 폐기됨)            |
| **암호화 세션**                                | 1회 핸드셰이크 후 세션 재활용 (Session Resumption) 가능 |

---

## ✅ 핵심 요약

| 항목        | 요약                                        |
| :-------- | :---------------------------------------- |
| **HTTP**  | 비연결형 평문 전송, 보안 없음                         |
| **HTTPS** | TLS 기반 암호화 통신, 인증서 기반 보안                  |
| **핵심 차이** | HTTPS는 **기밀성, 무결성, 인증**을 보장               |
| **결론**    | 모든 현대 웹 서비스는 HTTPS로 전환 중 (SSL은 더 이상 사용 X) |

---

> ✅ **한줄 요약:**
>
> * **HTTP**는 “빠르지만 불안한 평문 통신”
> * **HTTPS**는 “느리지만 안전한 암호화 통신”
>   → 오늘날 웹은 **HTTP → HTTPS로 완전 전환**이 표준이다.

```

---
