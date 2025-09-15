# 클럭 & CPI

## 클럭 (Clock)

- CPU 동자을 동기화하는 기본 박자(시간 단위)
- 클럭 주파수(Hz) : 1초 동안 바생하는 클럭 사이클 수
- 클럭 주기 : 한 사이클이 걸리는 시간
- 클럭은 CPU가 얼마나 빠르게 일을 시작할 수 있는가를 결정

<br>

## CPI (Cycles Per Instruction)

- 명령어 하나를 실행하는 데 필요한 평균 클럭 사이클 수
- 명령어 종류마다 소요시간 다름
    - 단순 연산 (ADD, SUB) → 1 사이클
    - 메모리 접근 (LOAD, STORE) → 수 사이클
    - 곱셈 / 나눗셈 → 더 많은 사이클
    - 분기 (Branch) → 파이프라인 실패 시 추가 지연 발생
- 평균 CPI : 프로그램 전체 실행에서 사용된 총 사이클 ÷ 총 명령어 수

### CPU 실행 시간 관계식

$$
CPU Time = \frac{InstructionCount * CPI}{Clock Frequency}
$$

- Instruction Count : 실행되는 명령어 수
- CPI : 명령어 당 평균 사이클 수
- Clock Cycle Time : 한 사이클의 시간