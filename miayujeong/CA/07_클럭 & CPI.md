## 📖 Clock & CPI

### Clock (클럭)

> 컴퓨터의 모든 부품이 움직이는 시간 단위
> 
- 컴퓨터는 clock pulse에 맞춰 명령어를 처리함
- 각 명령어 실행의 단계는 클럭 사이클로 나뉘어 처리됨
    - 클럭은 명령어 처리의 최소 단위 시간

#### 클럭 주기 (Clock period)

- 한 클럭 사이클에 걸리는 시간
- 단위: ns, ps 등

#### 클럭 속도 (Clock frequency, Clock rate)

- 클럭 주기의 역수  
    = 시간당 클럭 사이클이 몇 번 발생하는지
- 단위: Hz
    - 1초에 클럭이 몇 번 반복되는지
    - 1초에 클럭 100번 = 클럭 속도가 100Hz

#### 동적 클럭 관리

- 클럭 속도는 항상 일정하지 않고, 부하와 발열/전력 상황에 따라 동적으로 조절됨
- (ex)  i7 CPU의 클럭수는 **평균** 2.5GHz, **최고** 4GHz

<br>

### CPI

> CPI (Clock cycles per Instruction)

- 명령어 하나 실행에 평균적으로 필요한 cycle 수
- 명령어 종류마다 필요한 cycle 수가 다르기 때문에, CPU 전체의 성능을 분석하기 위해서는 평균 CPI 사용
- CPU 하드웨어에 의해 결정됨

<br>

## 📖 CPU 성능 측정

### CPU 성능에 영향을 주는 요소

- Instruction Count (IC): 알고리즘, 프로그래밍 언어, 컴파일러에 의해 결정
- CPI: CPU 아키텍처 (파이프라인, 캐시, 분기 예측 등)에 의해 결정
- Clock rate: 하드웨어 (클럭 속도)

<br>

### CPU의 성능

> CPU 성능은 어떤 관점에서 평가할 수 있을까?

- CPU의 성능은 다양한 관점에서 평가할 수 있음
- 시간 (응답시간, 실행시간), 시간당 작업량 등등…
- 보통 시간 기준으로

#### 실행시간을 기준으로 CPU의 성능

```
Performance = 1 / Execution Time
```

- 성능은 실행시간의 역수
- 서로 다른 CPU 2개가 있을 때, 상대적으로 실행시간 짧을수록 성능이 상대적으로 좋다고 말할 수 있다

<br>

### 클럭과 CPU 성능

#### 클럭과 CPU 성능

- 클럭 신호가 빨라짐 = 빨리 움직임 = 명령어 사이클을 더 빠르게 반복함
- 클럭 속도 = CPU 속도 단위로 볼 수 있음
- 클럭 속도 높을수록 CPU 성능 무조건 좋아지는가? 아님
    - 클럭 속도 높아지면 → 발열 문제 더 심해질 수 있음

#### CPU 실행시간 결정 

```
CPU Time = Instruction Count × CPI × Clock Cycle Time
         = (Instruction Count × CPI) / Clock Rate
```

- 총 클럭 사이클 수 = Instruction Count × CPI
- 클럭 주기 (Clock Cycle Time)은 Clock Rate의 역수 

⇒ 성능(CPU 실행시간)은 단순히 클럭 속도로 결정되지 않고, IC, CPI, 클럭 속도의 조합으로 결정됨