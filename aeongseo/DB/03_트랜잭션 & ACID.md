## 트랜잭션 & ACID

### 트랜잭션

1. 개념
    - 데이터베이스에서 하나의 논리적 작업 단위(Logical Unit of Work)를 의미
    - 여러 개의 SQL 연산을 하나의 묶음으로 처리하는 개념
    - ⇒ 데이터베이스 상태를 일관되게 유지하기 위해 수행되는 하나 이상의 데이터 조작 연산 집합
2. 트랜잭션의 상태(Transaction States)
    - Active : 트랜잭션이 수행중인 상태
    - Partially Committed : 마지막 명령을 실행했지만, 결과가 아직 DB에 완전히 반영되지 않은 상태
    - Committed : 모든 연산이 성공적으로 완료되어 DB에 반영된 상태
    - Failed : 실행 중 오류나 시스템 문제로 중단된 상태
    - Aborted : 트랜잭션이 실패하여 롤백된 상태

### ACID

1. 특성
    - 트랜잭션은 무결성과 일관성 보장 위해 ACID라는 4가지 성질 가져야 함
    - Atomicity(원자성)
        - All or Nothing
        - 트랜잭션의 모든 연산은 전부 수행되거나 전혀 수행되지 않아야 함
        - 실패 시 전체 롤백
    - Consistency(일관성)
        - Consistent State 유지
        - 트랜잭션 전후로 데이터의 제약조건(무결성 제약, 외래키 등)이 항상 만족되어야 함
    - Isolation(고립성)
        - 독립적 실행
        - 동시에 수행되는 여러 트랜잭션은 서로의 중간 결과를 볼 수 없어야 함
        - 동시성 제어(concurrency control) 필요
    - Durability(지속성)
        - 영속적 반영
        - 트랜잭션이 커밋되면 그 결과는 시스템 장애가 발생해도 유지되어야 함
        - 로그와 백업을 통해 보장
2. ACID 보장하는 기술적 메커니즘
    
    
    | ACID 성질 | 관련 기술 |
    | --- | --- |
    | Atomicity | Undo Log, Rollback |
    | Consistency | 제약조건(Constraints), 트리거(Trigger), 무결성 규칙 |
    | Isolation | Locking, MVCC (다중 버전 동시성 제어) |
    | Durability | Write-Ahead Logging (WAL), Checkpoint |