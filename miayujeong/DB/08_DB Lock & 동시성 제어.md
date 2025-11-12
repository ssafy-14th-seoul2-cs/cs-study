## 📖 DB Lock

- 데이터베이스에서 동시에 여러 트랜잭션이 같은 데이터를 수정하거나 읽는 것으로 인해 데이터의 일관성이 깨지는 것을 막기 위해 사용하는 제어 매커니즘
- 트랜잭션이 데이터에 접근할 때 Lock을 걸고, 작업이 끝나면  Lock 해제 (Unlock)
- Lock을 통해 동시성과 일관성 간 균형을 맞출 수 있음

### Shared Lock (S-Lock, 공유 락)

- 데이터를 읽기(Read) 전용으로 접근할 때 설정되는 락
- 여러 트랜잭션이 동시에 공유 락 획득 가능
- 공유 락이 걸린 데이터에는 쓰기(Write) 불가

<br>

### Exclusive Lock (X-Lock, 배타 락)

- 데이터를 쓰기 (Update, Delete, Insert)하기 위해 설정되는 락
- 하나의 트랜잭션만 접근 가능
- 배타 락이 걸린 데이터에는 다른 트랜잭션은 읽기도 불가 (배타 락이 해제될 때까지 대기)

<br>

### Intention Lock (의도 락)

- 상위 객체 (테이블, 페이지)에 하위 레벨에 락 걸 예정임을 표시
- IS (Intention Shared), IX(Intention Exclusive) 등으로 표현
- 락 충돌을 빠르게 탐지하고 병행 제어 효율화

<br>

### Lock 단위 (Lock Granularity)

: 락을 거는 적용 범위(단위)

| **락 단위** |  | **장점** | **단점** |
| --- | --- | --- | --- |
| **Row-level Lock** | 한 행(row) | 병행성(동시 처리 능력)이 높음 | 관리 오버헤드가 큼 |
| **Page-level Lock** | 페이지(8KB~16KB 등 단위) | 병행성과 관리 효율이 균형 잡힘 | 일부 불필요한 락 발생 가능 |
| **Table-level Lock** | 테이블 전체 | 관리가 간단하고 구현이 쉬움 | 병행성이 매우 낮음 |

<br>

## 📖 동시성 제어 (Concurrency Control)

### 문제 상황

| 문제 유형 | 설명 | 예시 |
| --- | --- | --- |
| **Dirty Read** | 커밋되지 않은 데이터를 읽음 | T1이 수정 중인데 T2가 읽음 |
| **Non-repeatable Read** | 같은 데이터 두 번 읽었는데 값 달라짐 | 중간에 T2가 UPDATE |
| **Phantom Read** | 같은 조건 SELECT했는데 행 수 달라짐 | 중간에 T2가 INSERT/DELETE |

<br>

### 2단계 로킹 프로토콜 (Two-Phase Locking, 2PL)

- 동시성 제어 문제를 해결하기 위한 lock-based protocol 중 하나
- 직렬 가능성 (Serializability) 보장 방식
- 트랜잭션이  Lock을 걸고 해제하는 과정을 두 단계로 나눔

#### 확장 단계 (Growing Phase)

- 트랜잭션이 Lock 획득만 가능, 해제 불가
- 필요 데이터를 모두 Lock할 때까지 확장

#### 축소 단계 (Shrinking Phase)

- Lock을 해제만 가능, 새 Lock 획득 불가
- Unlock 발생하는 순간부터 축소 단계로 진입

<br>

### 데드락 (Deadlock)

- 두 개 이상의 트랜잭션이 서로가 보유한 락을 기다리는 상황
- 2PL은 직렬 가능성 보장하지만, 데드락 가능성 있음

#### 데드락 해결 방법

| 방식 | 설명 |
| --- | --- |
| **Wait-Die / Wound-Wait** | 타임스탬프 기반 선후 관계로 해결 (오래된 트랜잭션이 우선) |
| **Timeout** | 일정 시간 기다렸다가 해제 후 Rollback |
| **Deadlock Detection** | 주기적으로 Wait-for Graph 점검하여 사이클 발생 시 강제 종료 |
