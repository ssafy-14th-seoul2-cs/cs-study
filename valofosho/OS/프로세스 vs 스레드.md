# 프로세스 vs 스레드

## 프로세스(Process)
- 실행 중인 프로그램의 인스턴스  
- 독립된 메모리 공간(코드, 데이터, 힙, 스택)을 가짐  
- 다른 프로세스와 자원을 공유하지 않음 → 안정적이지만 통신이 복잡  

---
<img width="535" height="265" alt="Process_vs_thread1" src="./images/Process_vs_thread1.png" /><br/>

## 스레드(Thread)
- 프로세스 내 실행 흐름의 단위  
- 프로세스의 코드/데이터/힙은 공유, 스택/레지스터/PC는 개별  
- 경량 프로세스(Lightweight Process, LWP)라고도 불림  
- 다수의 스레드가 협력하여 프로세스의 작업을 수행  

---

## 메모리 구조
- **프로세스**: 코드, 데이터, 힙, 스택 → 모두 독립적  
- **스레드**: 코드/데이터/힙 공유, 스택/레지스터/PC는 개별  

---

## 멀티프로세스 vs 멀티스레드

<img width="535" height="265" alt="Process_vs_thread3" src="./images/Process_vs_thread3.png" /><br/>

### 멀티프로세스 (Multi-Processing)
- 장점: 안정성 높음 (한 프로세스 오류가 다른 프로세스에 영향 X)  
- 단점: IPC(프로세스 간 통신) 오버헤드 큼, Context Switching 비용 큼  

### 멀티스레드 (Multi-Threading)
<img width="535" height="265" alt="Process_vs_thread2" src="./images/Process_vs_thread2.png" /><br/>

- 장점: 자원 공유로 빠른 통신, 생성/종료 비용 낮음  
- 단점: 동기화 필요, 한 스레드 오류가 프로세스 전체에 영향  

---

## 스레드 관련 문제
- **Race Condition**: 여러 스레드가 동시에 같은 데이터에 접근 → 실행 순서에 따라 결과 달라짐  
- **Deadlock**: 두 실행 단위가 서로 자원을 기다리며 무한 대기  

---

## Context Switching (문맥 전환)
- 실행 단위 간 전환 시 상태 저장/복원 필요 → 비용 발생  
- **프로세스 전환 > 스레드 전환** (비용이 더 큼)  

---

## 하이퍼스레딩 (Hyper-Threading)
- Intel SMT(Simultaneous Multi-Threading) 기술  
- 하나의 물리적 코어를 여러 논리 스레드로 활용  
- 유휴 자원 활용, 멀티태스킹 성능 개선  

---

## 언어/런타임 제약 (Python GIL)
- CPython의 GIL(Global Interpreter Lock): 한 시점에 하나의 스레드만 Python 바이트코드 실행  
- CPU 바운드 작업: 멀티스레딩 효과 제한적  
- I/O 바운드 작업: 멀티스레딩 유리  
- 대안: multiprocessing 모듈  

---

## 프로세스 vs 스레드 (핵심 비교표)

| 구분 | 프로세스(Process) | 스레드(Thread) |
|------|-------------------|----------------|
| 실행 단위 | 실행 중인 프로그램 인스턴스 | 프로세스 내 실행 흐름 단위 |
| 메모리 구조 | 코드/데이터/힙/스택 모두 독립 | 코드/데이터/힙 공유, 스택은 개별 |
| 생성/종료 비용 | 높음 (무겁다) | 낮음 (가볍다) |
| 통신 | IPC 필요, 복잡하고 느림 | 공유 메모리로 빠르고 단순 |
| Context Switching | 무겁고 비용 큼 | 가볍고 비용 낮음 |
| 안정성 | 한 프로세스 오류 → 다른 프로세스 영향 없음 | 한 스레드 오류 → 프로세스 전체 영향 |
| 활용 사례 | 안정성/격리 필요한 서버, OS 서비스 | 동시성·병렬성 필요한 애플리케이션 |
