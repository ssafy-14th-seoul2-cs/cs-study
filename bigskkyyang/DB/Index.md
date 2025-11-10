# 인덱스 (Index)

## 인덱스란?

**인덱스**는 데이터베이스에서 데이터를 빠르게 찾기 위한 **목차**와 같은 개념

## 인덱스의 구조

### 가장 많이 사용되는 구조: B-Tree

```
                [50]
              /      \
         [25]          [75]
        /    \        /    \
    [10]   [30]   [60]   [90]
```

- 데이터를 **트리 구조**로 정리
- 빠른 검색이 가능 (O(log N))
- MySQL, PostgreSQL 등 대부분의 DB에서 기본으로 사용

### B-Tree의 장점

1. **검색이 빠름**: 데이터가 정렬되어 있어서 빠르게 찾을 수 있음
2. **범위 검색에 유리**: "30 이상 70 이하" 같은 조건 검색에 효과적
3. **균형 잡힌 구조**: 트리의 높이가 일정하게 유지됨

## 인덱스의 종류

### 1. 기본 인덱스 (Primary Key Index)

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,  -- 자동으로 인덱스 생성
    name VARCHAR(100)
);
```

- Primary Key를 지정하면 **자동으로 생성**
- 각 행을 고유하게 식별
- 가장 빠른 검색 성능

### 2. 단일 컬럼 인덱스 (Single Column Index)

```sql
-- name 컬럼에 인덱스 생성
CREATE INDEX idx_name ON users(name);
```

- 하나의 컬럼에 대한 인덱스
- 해당 컬럼으로 검색할 때 빠름

### 3. 복합 인덱스 (Composite Index)

```sql
-- name과 age 컬럼을 함께 인덱스로 생성
CREATE INDEX idx_name_age ON users(name, age);
```

- 여러 컬럼을 묶어서 인덱스 생성
- **컬럼 순서가 중요함**

**복합 인덱스 사용 규칙:**
```sql
-- idx_name_age(name, age) 인덱스가 있을 때

-- 인덱스 사용 O
SELECT * FROM users WHERE name = '김철수';
SELECT * FROM users WHERE name = '김철수' AND age = 30;

-- 인덱스 사용 X (name이 조건에 없음)
SELECT * FROM users WHERE age = 30;
```

전화번호부를 생각하면 쉽습니다:
- (성, 이름) 순서로 정렬되어 있으면
- "김"씨를 찾기는 쉽지만
- "철수"라는 이름만으로는 찾기 어려움

### 4. 고유 인덱스 (Unique Index)

```sql
CREATE UNIQUE INDEX idx_email ON users(email);
```

- 중복된 값을 허용하지 않음
- 이메일, 주민등록번호 같은 고유 값에 사용

## 인덱스의 단점

### 1. 저장 공간 차지

- 인덱스도 데이터이므로 **추가 저장 공간**이 필요
- 테이블이 클수록 인덱스도 커짐

### 2. 쓰기 작업이 느려짐

데이터를 추가, 수정, 삭제할 때:
```sql
INSERT INTO users (name, age) VALUES ('김철수', 30);
```

1. 실제 테이블에 데이터 추가
2. **인덱스도 업데이트** 필요
3. 인덱스를 정렬된 상태로 유지

인덱스가 많을수록 쓰기 작업이 느려진다.

### 3. 잘못 사용하면 오히려 느려짐

- 인덱스가 너무 많으면 오히려 성능 저하
- 인덱스 선택하는 데 시간이 걸림
- 모든 컬럼에 인덱스를 만들면 안 됨

## 인덱스 사용 시 주의사항

### 1. 선택도(Selectivity)가 높은 컬럼에 생성

**선택도**: 전체 데이터 중 고유한 값의 비율

```sql
-- 좋은 예: 이메일 (선택도 높음)
-- 100만 명 회원, 100만 개의 서로 다른 이메일
CREATE INDEX idx_email ON users(email);

-- 나쁜 예: 성별 (선택도 낮음)
-- 100만 명 회원, 2가지 값만 존재 (남/여)
CREATE INDEX idx_gender ON users(gender);  -- 비효율적
```

선택도가 낮으면 인덱스를 사용해도 효과가 적다.

### 2. 함수나 연산 사용 시 인덱스 무효화

```sql
-- 인덱스 사용 X
SELECT * FROM users WHERE YEAR(created_at) = 2024;
SELECT * FROM users WHERE age + 1 = 31;

-- 인덱스 사용 O
SELECT * FROM users WHERE created_at >= '2024-01-01' 
                     AND created_at < '2025-01-01';
SELECT * FROM users WHERE age = 30;
```

컬럼에 함수나 연산을 적용하면 인덱스를 사용할 수 없다.

### 3. NULL 값 처리

```sql
-- 인덱스 사용에 영향
SELECT * FROM users WHERE deleted_at IS NULL;
```

DB마다 NULL 처리 방식이 다르므로 확인이 필요하다.

### 4. 복합 인덱스의 컬럼 순서

```sql
-- 인덱스: (name, age, city)

-- 인덱스 사용 O
WHERE name = '김철수'
WHERE name = '김철수' AND age = 30
WHERE name = '김철수' AND age = 30 AND city = '서울'

-- 인덱스 사용 X (name이 없음)
WHERE age = 30
WHERE city = '서울'
WHERE age = 30 AND city = '서울'
```

**앞쪽 컬럼부터 순서대로** 사용해야 인덱스가 효과적이다.

## 인덱스 생성 가이드

### 언제 인덱스를 만들어야 할까?

**만들어야 하는 경우:**
- WHERE 절에 자주 사용되는 컬럼
- JOIN 조건에 사용되는 컬럼
- ORDER BY에 자주 사용되는 컬럼
- 데이터가 많은 테이블 (최소 수만 건 이상)
- 읽기 작업이 쓰기 작업보다 훨씬 많은 경우

**만들지 말아야 하는 경우:**
- 데이터가 적은 테이블 (수천 건 이하)
- 쓰기 작업이 매우 빈번한 컬럼
- 선택도가 낮은 컬럼 (값의 종류가 적음)
- 거의 사용하지 않는 컬럼