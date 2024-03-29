# CPU 스케줄링

CPU 스케줄링(`scheduling`)은 다중 프로그램 운영체제의 기본이다!

- 운영체제는 CPU를 프로세스 간에 교환함으로써, 컴퓨터를 보다 생산적으로 만든다!
- “프로세스 스케줄링”과 “스레드 스케줄링”은 상호 교환적으로 사용되곤 한다.
    - 이 장에서는 일반적 스케줄링 개념을 논의할 때는 “프로세스 스케줄링”을 사용하고,
    - 스레드에 국한된 개념을 가리키는 경우에는 “스레드 스케줄링”이라는 용어를 사용하기로 한다.

<br/>

<br/>

# 💡 기본 개념 _Basic Concepts

앞서 **다중 프로그래밍의 목적**은 **CPU 이용률을 최대화**하기 위해 항상 실행 중인 프로세스를 가진다고 했다.

- 코어가 하나인 시스템에서, 하나의 프로세스가 I/O 요청을 기다린다면?
- CPU는 시간을 낭비하며, 어떤 유용한 작업도 진행하지 못한다! → **CPU 이용률 낭비!**

<br/>

<br/>

다중 프로그래밍은 이러한 낭비를 줄이고, 시간을 생산적으로 활용하려 한다.

- 어떤 프로세스가 대기해야 할 경우, **CPU를 회수**해 다른 프로세스에 할당하고, 이러한 패턴은 반복된다.

<br/>

<br/>

## CPU-I/O 버스트 사이클

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-IO-burst.png" width = 600/>


CPU 스케줄링의 성공은 프로세스들의 다음과 같은 관찰된 성질에 의해 좌우된다.

- 프로세스 실행은 CPU 실행과 I/O 대기의 **사이클**로 구성된다.
    
    → 프로세스들은 이들 두 상태 사이를 교대로 왔다 갔다 한다.
    
- 프로세스 실행은 **CPU 버스트(burst)**로 시작된다.

    → 이후, **I/O 버스트**가 발생하고, 두 버스트의 발생이 반복된다.
    

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-burst.png" width = 600/>


CPU 버스트들의 지속 시간을 광범위하게 측정한 결과는 아래와 같다.

- 일반적으로 지수형 또는 초지수형으로 특성화된 곡선을 찾아볼 수 있다.
- **짧은 CPU 버스트가 많이 있으며, 긴 CPU 버스트는 적다.**
    - I/O 중심의 프로그램 : 짧은 CPU 버스트를 많이 가짐
    - CPU 지향 프로그램 : 다수의 긴 CPU 버스트를 가짐

<br/>

<br/>

## CPU 스케줄러

- CPU가 유휴 상태가 될 때마다, OS는 준비 큐의 프로세스 중 하나를 선택해 실행한다.
    - 이 선택은 **CPU 스케줄러**에 의해 수행된다.
    - ***이때, 준비 큐는 반드시 FIFO 방식의 큐가 아니어도 되는 것에 유의하자!***

<br/>

<br/>

## 선점 및 비선점 스케줄링

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/process-state.png" width = 600/>


CPU 스케줄링 결정은 다음의 네 가지 상황에서 발생할 수 있다.

1. 한 프로세스가 실행 상태에서 대기 상태로 전환될 때 (`running → waiting`) : **비선점**
    
    → ex) I/O 요청을 대기해야 할 때
    
2. 프로세스가 실행 상태에서 준비 완료 상태로 전환될 때 (`running → ready`) : **선점**
    
    → ex) 인터럽트가 발생할 때
    
3. 프로세스가 대기 상태에서 준비 완료 상태로 전환될 때 (`waiting → ready`) : **선점**
    
    → ex) I/O 작업 완료 시
    
4. 프로세스가 종료할 때 (`running → terminated`) : **비선점**

<br/>

<br/>

> 비선점 스케줄링
> 
- 아래의 두 경우, CPU를 점유한다
    - CPU가 한 프로세스에 할당되면 프로세스가 종료하든지 (위 상황에서 4번)
    - 또는 대기 상태로 전환해 CPU를 방출하든지! (위 상황에서 1번)

<br/>

<br/>

> 선점 스케줄링
> 
- 데이터가 다수의 프로세스에 의해 공유될 때 **경쟁 조건을 초래**할 수 있다.
- 운영체제 커널 설계에 영향을 줄 수 있다.
    - 시스템 콜을 처리할 동안, 중요한 커널 자료가 변경될 경우 커널이 프로세스를 선점!
    - 단, 이러한 기능을 지원하려면 설계가 복잡해지지만, 비선점 커널은 좋은 모델은 아니다.

<br/>

<br/>

### 디스패처

CPU 스케줄링 기능에 포함된 또 하나의 요소는 **디스패처(dispatcher)**이다.

<br/>

> 디스패처란?
> 
- CPU 코어의 제어를 CPU 스케줄러가 선택한 프로세스에게 주는 모듈
- 아래의 작업들을 포함한다.
    - 프로세스 사이에서의 문맥 교환
    - 사용자 모드로 전환
    - 프로그램을 다시 시작하기 위해 적절한 위치로 이동(jump)

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/dispatch-latency.png" width = 600/>


- 디스패치 지연(`dispatch latency`)란?
    - 디스패처가 하나의 프로세스를 정지하고, 다른 프로세스의 수행을 시작하는데 까지 걸린 시간
    - ***디스패처는 모든 프로세스의 문맥 교환시 호출된다. → 속도가 생명!***
    - 이때, 중요한 건 “문맥 교환이 얼마나 자주 발생하는가?” 이다.

<br/>

<br/>

- Linux에서 제공하는 문맥 교환 횟수를 얻는 명령어 
    ```bash
    $ vmstat 1 3 #1초 지연 단위로 3줄의 출력을 제공!
    ```
    

<br/>

<br/>

- 특정 프로세스에 대한 문맥 교환 횟수를 결정하기
    
    ```bash
    $ cat /proc/2166/status #pid = 2166인 프로세스에 대한 통계를 보기
    
    # 정상 출력 결과
    # voluntary_ctxt_switches 150
    # nonvoluntary_ctxt_switches 8
    ```
    
    
    - 위 결과에서 **자발적 문맥 교환과 비자발적 문맥 교환**의 차이점에 주목하자!
    - ***자발적*** 문맥 교환
        - 현재 사용 불가능한 자원을 요청하여 프로세스가 CPU 제어를 포기한 경우
    - ***비자발적*** 문맥 교환
        - 타임 슬라이스가 만료되었거나 다른 프로세스에게 CPU를 빼았겼을 경우


<br/>

<br/>

# 💡 스케줄링 기준


CPU 스케줄링 알고리즘을 비교하기 위한 여러 기준이 존재한다.

1. **CPU 이용률**(`utilization`)
    1. 가능한 한 CPU를 최대한 바쁘게 유지하기를 원한다.
    
2. **처리량**(`throughput`)
    1. 작업량 측정의 한 방법은 단위 시간당 완료된 프로세스의 개수로, 이를 **처리량**이라고 한다.
    
3. **총처리 시간**(`turnaround time`)
    1. 프로세스를 실행하는 데 소요된 시간이 중요할 수도 있다.
    2. 프로세스의 제출 시간과 완료 시간의 간격을 총처리 시간이라고 한다.
    3. **준비큐에서 대기한 시간 + CPU에서 실행하는 시간 + I/O 시간을 합한 시간**

1. **대기 시간**(`waiting time`)
    1. 스케줄링 알고리즘은 단지 프로세스가 준비 큐에서 대기하는 시간의 양에만 영향을 준다.
    2. **준비큐에서 대기하면서 보낸 시간의 합**

1. **응답 시간**(`response time`)
    1. 대화식 시스템의 경우 총처리 시간이 최선의 기준이 아닐 수 있다.
    2. 따라서 또 다른 기준은 하나의 요구를 제출한 후, **첫 번째 응답이 나올 때까지의 시간**이다.

<br/>

<br/>


> 🌱 CPU 이용률과 처리량을 최대화하고 총처리 시간, 대기 시간, 응답 시간을 최소화하는 것이 바람직!
> 
> 대부분의 경우, 평균 측정 시간을 최적화하려 하지만 때로는 평균보단 최소, 최댓값을 최적화하는 것이 바람직 할 수도 있다!

<br/>

<br/>

# 💡 스케줄링 알고리즘

> CPU 스케줄링은 준비 큐에 있는 어느 프로세스에 CPU 코어를 할당할 것인지를 결정하는 문제를 다룬다.
> 

<br/>


## 선입 선처리 스케줄링 (FCFS: First come, First served Scheduling)

- 가장 간단한 CPU 스케줄링 알고리즘은 **선입 선처리(FCFS) 스케줄링 알고리즘**이다.
    - CPU를 먼저 요청하는 프로세스가 CPU를 먼저 할당받는다.
    - 이 정책의 구현은 FIFO 큐로 쉽게 관리할 수 있다!

<br/>

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/FCFS-gantt.png" width = 600/>



>🌱 선입 선처리 정책에서 평균대기 시간은 일반적으로 최소가 아니다.

<br/>

<br/>

> 추가로 동적 상황에서 선입 선처리 스케줄링의 성능을 고려해보자!
> 
>1. CPU 중심 프로세스가 CPU를 점유하는 동안, 다른 프로세스들은 준비 큐에서 대기한다.
>2. 자신의 작업을 끝낸 프로세스는 I/O 장치로 이동한다.
>3. 모든 I/O 중심 프로세스들은 매우 짧은 CPU 버스트를 가지기에 신속히 끝낸다.
>4. 이후, 준비 큐에서 새로운 CPU 중심 프로세스가 긴 작업을 진행하는 동안 수많은 I/O 프로세스들은 대기한다.
>
>→ 이처럼 **모든 다른 프로세스들이 하나의 긴 프로세스가 CPU를 양도하기를 기다리는 것**을 **호위 효과**라고 한다.

<br/>

<br/>

>🌱 FCFS 스케줄링은 비선점형이라는 것을 주의하자.
>
>대화형 시스템은 각 프로세스가 규칙적인 간격으로 CPU의 몫을 얻는 것이 중요하기 때문에 맞지 않다.


<br/>

<br/>


## 최단 작업 우선 스케줄링 (SJF : Shortest-Job-First Scheduling)

- 최단 작업 우선(SJF) 알고리즘은 각 프로세스에 CPU 버스트 길이를 연관시킨다.
    - CPU가 이용 가능해지면, **가장 작은 CPU 버스트를 가진 프로세스에 할당**한다.


<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/SJF-gantt.png" width = 600/>
    

- SJF 스케줄링 알고리즘은 주어진 프로세스 집합에 대해 최소 평균대기 시간을 줄인다. → **최적임을 증명!**
    - 짧은 프로세스를 긴 프로세스의 앞으로 이동할 수록 평균 대기 시간을 줄일 수 있다.
    - **단, 다음 CPU 버스트의 길이를 알 방법이 없기 때문에 구현할 수 없다.**

<br/>

<br/>

- 한 가지 접근 방식은 SJF 스케줄링과 근사한 방법을 사용하는 것이다!
    - 다음 **CPU 버스트**의 길이를 알 수는 없으나, **그 값을 예측해보는 것이다.**
    - 다음 CPU 버스트가 이전의 버스트와 길이가 비슷하다고 기대하고, 근삿값을 계산하여 짧은 값을 고른다.


<br/>

<br/>

- SJF 알고리즘은 선점형이거나 비선점형일 수 있다.
    - **선점형** : 현재 실행하는 프로세스보다 새로운 프로세스의 버스트가 짧으면 프로세스를 선점
    - **비선점형** : 현재 실행하는 프로세스가 자신의 CPU 버스트를 끝내도록 허용
    - SJF 알고리즘은 때때로 **최소 잔여 시간 우선(shortest remaining time first)** 스케줄링이라 부른다.

<br/>

<br/>

## 라운드 로빈 스케줄링 (Round-Robin Scheduling)

- **라운드 로빈(RR)** 스케줄링 : FCFS와 유사하지만, 프로세스들 사이를 옮겨 다닐 수 있도록 **선점이 추가**된다.
    - 또한 **시간 할당량**, 또는 **타임 슬라이스**라고 하는 작은 단위의 시간을 정의한다.
    
<br/>

<br/>


- 라운드 로빈 스케줄링 구현
    - 준비 큐를 선입선출로 동작하게 만든다.
    - 새로운 프로세스들은 준비 큐의 꼬리에 추가한다.
    - CPU 스케줄러는 준비큐에서 프로세스를 차례대로 선택해 시간 할당량만큼 할당한다.

<br/>

<br/>

**라운드 로빈에서 발생할 수 있는 사례**

1. 프로세스의 CPU 버스트가 시간 할당량보다 작을 경우
    - 프로세스 자신이 CPU를 자발적으로 방출하도록 할 것
    - 이후, 다음 프로세스로 진행하기
2. 실행 중인 프로세스의 CPU 버스트가 시간 할당량보다 긴 경우
    - 타이머가 끝나면 인터럽트가 발생하므로, 문맥교환이 일어나 준비 큐의 꼬리에 다시 넣는다.
    - 이후, CPU 스케줄러는 다음 프로세스를 선택해 진행한다.

<br/>

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/RR-gantt.png" width = 600/>


<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/RR-quantum.png" width = 600/>

> RR 알고리즘의 성능은 시간 할당량의 크기에 매우 많은 영향을 받는다.
> 
- 시간 할당량이 매우 클 경우 → FCFS 정책과 같다.
- 시간 할당량이 매우 작을 경우 → 매우 많은 문맥 교환이 발생, 프로세스의 실행이 느려짐

<br/>

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/RR-turnaround.png" width = 600/>

<br/>

<br/>

> 또한 총처리 시간 또한 시간 할당량의 크기에 좌우된다.
> 
- 한 프로세스 집합의 평균 총처리 시간은 시간 할당량의 크기가 증가해도 반드시 개선되지는 않는다.
    - 일반적으로, 대부분의 프로세스가 단일 시간 할당량 안에 버스트를 끝내야 개선된다.
    - 따라서 CPU 버스트의 80%는 시간 할당량보다 짧아야 한다.

<br/>

<br/>

## 우선순위 스케줄링

- SJF 알고리즘은 일반적인 **우선순위 스케줄링** 알고리즘의 특별한 경우이다!
    - 일반적으로 CPU는 가장 높은 우선순위를 가진 프로세스에 할당된다.
    - 우선순위가 같으면 FCFS 순서로 스케줄된다.

<br/>

<br/>

- 우리가 ***높은*** 우선순위와 ***낮은*** 우선순위에 의해 스케줄링을 논의하고 있음에 유의하자!
    - 일반적으로 0이 최상위 또는 최하위 우선순위인가에 대한 합의는 없다. → 시스템마다 다르다.
    
<br/>

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/prior-gantt.png" width = 600/>
        

<br/>

<br/>

> 우선순위 스케줄링은 선점형이거나 비선점형이 될 수 있다.
> 
- **선점형** : 새로 도착한 프로세스가 현재 실행 중인 프로세스보다 우선 순위가 높으면 선점
- **비선점형** : 단순히 준비완료 큐의 머리 부분에 새로운 프로세스를 넣는다.

<br/>

<br/>

> 우선순위 스케줄링 알고리즘의 주요 문제 : 무한 봉쇄(indefinite blocking) 또는 기아 상태(starvation)
> 
- 실행 준비는 되었으나, 우선 순위가 낮아 무한히 대기하는 경우가 발생할 수 있다!
- 이를 해결하는 방안 : **노화(aging)**
    - **오랫동안 대기하는 프로세스들의 우선순위를 점진적으로 증가시킨다.**

<br/>

<br/>

> 우선순위 스케줄링의 다른 옵션 : ***라운드로빈 + 우선순위 스케줄링***
> 
- 우선 순위가 높은 프로세스를 실행하고, 우선순위가 같으면 라운드 로빈 방식으로 스케줄한다.

<br/>

<br/>

## 다단계 큐 스케줄링

> 큐가 관리되는 방식에 따라 우선순위가 가장 높은 프로세스를 결정하기 위해 O(n) 검색이 필요할 수 있다.
> 

<br/>

<br/>

- 실제로 아래 그림과 같이 우선순위마다 별도의 큐를 갖는 것이 더 쉬울 때도 있다.
    - 우선순위가 가장 높은 큐에서 프로세스를 스케줄 하는 방식으로 동작한다.
    
<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/separate-queue.png" width = 600/>
    

<br/>

<br/>

- **다단계 큐**라고 하는 이 방식은 ***라운드로빈 + 우선순위 스케줄링 방식으로 동작***한다.
    - 프로세스 유형에 따라 프로세스를 여러 개의 개별 큐로 분할하기 위해 사용할 수도 있다.
    - 흔히 **포그라운드**(대화형) 프로세스와 **백그라운드**(배치) 프로세스를 구분한다.
        
        
<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/multilevel-queue.png" width = 600/>
        

<br/>

<br/>

- 위 두 가지 유형의 프로세스는 응답 시간 요구 사항이 달라 스케줄링 요구 사항이 다를 수 있다?
    - 또한 포그라운드 프로세스는 백그라운드 프로세스보다 높은 우선순위를 가질 수 있다.
    - 각 큐에는 자체 스케줄링 알고리즘이 있을 수 있고, 큐와 큐 사이에 스케줄링도 필요하다.

<br/>

<br/>

## 다단계 피드백 큐 스케줄링

- 다단계 큐 스케줄링은 프로세스가 영구적으로 하나의 큐에만 할당된다.
    
    = 다른 큐로 이동하지 않는다 → 오버헤드가 적어 장점이지만 **융통성이 적다!**
    

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/multilevel-feedback-queue.png" width = 600/>

<br/>

<br/>

- ***대조적으로 다단계 피드백 큐 스케줄링은 프로세스가 큐들 사이를 이동하는 것을 허용한다.***
    - 프로세스들을 CPU 버스트 성격에 따라 나눈다.
    - CPU 시간을 너무 많이 사용하는 프로세스 → 낮은 우선순위 큐로 이동!
    - = I/O 중심 프로세스와 대화형 프로세스들을 높은 우선순위 큐로 넣음
    - **마찬가지로, 낮은 우선순위 큐에서 너무 오래 대기하면 높은 우선순위 큐로 이동 → 기아 상태 방지!**

<br/>

<br/>

- 일반적으로 다단계 피드백 큐 스케줄러는 다음의 매개변수에 의해 정의된다.
    - 큐의 개수
    - 각 큐를 위한 스케줄링 알고리즘
    - 한 프로세스를 높은 우선순위 큐로 올려주는 시기를 결정하는 방법
    - 한 프로세스를 낮은 우선순위 큐로 강등시키는 시기를 결정하는 방법
    - 프로세스에 서비스가 필요할 때 프로세스가 들어갈 큐를 결정하는 방법


<br/>

<br/>

# 💡 스레드 스케줄링

## 경쟁 범위

- 사용자 수준과 커널 수준 스레드의 차이 : ***“그들이 어떻게 스케줄 되느냐”***이다.

<br/>

<br/>

- **프로세스 경쟁 범위**(`PCS`)
    - 다대일과 다대다 모델을 구현하는 시스템 환경에서 스레드 라이브러리는 사용자 수준 스레드를 가용한 LWP(경량 프로세스) 상에서 스케줄한다.
    - **동일한 프로세스에 속한 스레드끼리 CPU를 경쟁**

<br/>

<br/>

- **시스템 경쟁 범위**(`SCS`)
    - **CPU 상에 어느 커널 스레드를 스케줄 할 것인지 결정하기 위해 사용**

<br/>

<br/>

>🌱 전형적으로 PCS는 우선순위에 따라 행해진다.
>
>→ 즉, 스케줄러는 가장 높은 우선순위를 가진 실행 가능한 프로세스를 실행한다.
>
>이때, PC는 통상 더 높은 우선순위의 스레드를 위하여 선점한다는 것을 주의해야 한다.

<br/>

<br/>

## PThread 스케줄링

- 스레드를 생성하며 `PCS` 또는 `SCS`를 지정할 수 있는 **POSIX Pthreads API**를 알아보자.
    - Pthreads는 아래와 같은 범위 값을 구분한다.
        - `PTHREAD SCOPE PROCESS` : PCS 스케줄링을 사용
        - `PTHREAD SCOPE SYSTEM` : SCS 스케줄링을 사용

<br/>

<br/>

- Pthread IPC는 경쟁 범위 정책의 정보를 얻어내고 지정하기 위해 아래 두 함수를 제공한다.
    - `pthread attr setscope(pthread attr t *attr, int scope)`
    - `pthread attr getscope(pthread attr t *attr, int *scope)`
    - 첫번째 매개변수 : 스레드를 위한 속성 집합을 가리키는 포인터를 저장
    - 두 번째 매개변수로는 경쟁 범위가 어떻게 지정되는가를 전달


<br/>

<br/>

## 💡 다중 처리기 스케줄링

> 지금까지의 논의는 단일 처리기 코어를 가진 시스템에서 CPU를 스케줄링하는 문제였다.
> 

<br/>

<br/>

- 만일 여러 개의 CPU가 사용 가능하다면?
    
    → 여러 스레드가 병렬로 실행될 수 있으므로 **부하 공유(load sharing)이 가능**해진다!

<br/>

<br/>
    

- 최신 컴퓨팅 시스템에서 다중 처리기는 아래의 시스템 아키텍처들에 사용할 수 있다.
    - 다중 코어 CPU
    - 다중 스레드 코어
    - NUMA 시스템
    - 이기종 다중 처리

<br/>

<br/>

## 다중 처리기 스케줄링에 대한 접근 방법

1. **비대칭 다중 처리(asymmetric multiprocessing)**
    - 하나의 처리기(마스터 서버)가 모든 스케줄링 결정과 다른 시스템의 활동을 취급하게 한다.
    - 오직 하나의 코어만 시스템 자료구조에 접근하여 자료 공유의 필요성을 배제하므로 간단하다!
    - **다만, 마스터 서버가 전체 시스템 성능을 저하시킬 수도 있다는 단점이 있다.**

<br/>

<br/>

1. **대칭 다중 처리(symmetric multi-processing, SMP)**
    - 다중 처리기를 처리하기 위한 표준 접근 방식으로, 각 프로세서는 스스로 스케줄링할 수 있다.
    - 이때, 스케줄 대상이 되는 스레드를 관리하기 위한 두 가지 전략이 있다.
        
        <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/organization-of-ready-queue.png" width = 600/> 
        
        1. 모든 스레드가 공통 준비 큐에 있을 수 있다.
        2. 각 프로세서는 자신만의 스레드 큐를 가진다.

<br/>

<br/>

- ***공통 준비 큐를 가질 경우***
    - 경쟁 조건이 생기지 않도록 두 개의 다른 프로세서가 동일한 스레드를 스케줄 하지 않아야 한다.
        - 이를 위해 경쟁 조건으로부터 락킹 기법의 하나를 사용할 수 있다.
        - 단, 큐에 대한 액세스에는 락 소유권이 필요하므로, 공유 큐의 성능이 저하될 수 있다.

<br/>

<br/>

- ***자신만의 큐를 가질 경우***
    - SMP를 지원하는 시스템에서 가장 일반적인 접근 방식
    - 각 프로세서의 실행 큐는 공유 실행 큐와 관련되어 발생할 수 있는 성능 문제를 겪지 않는다.
    - 또한 자신만의 프로세스별 큐가 있어 캐시 메모리를 보다 효율적으로 사용할 수 있다.
        - **단, 큐마다 부하의 양이 다를 수 있어 신경써야 한다.**

<br/>

<br/>

## 다중 코어 프로세서

> 현대 컴퓨터 하드웨어는 동일한 물리적인 칩 안에 여러 코어를 장착하여 **다중 코어 프로세서**이다.
> 

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/memory-stall.png" width = 600/> 

- 다중 코어 프로세서는 스케줄링 문제를 복잡하게 한다.
    - **메모리 스톨(memory stall)**이란?
    - 프로세서가 메모리에 접근할 때 데이터가 가용해지기를 기다리면서 많은 시간을 허비하는데,
    - 이는 최신 프로세서가 메모리보다 훨씬 빠른 속도로 작동하기 때문이다!

<br/>

<br/>

- 해결법
    
<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/multithreaded-multicore-system.png" width = 600/> 
    
    - 하나의 코어에 2개 이상의 **하드웨어 스레드**를 할당한다.
    - 하나의 하드웨어 스레드가 메모리를 기다리는 동안 다른 스레드로 전환하도록 한다.

<br/>

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/chip-multithreading.png" width = 600/> 

- 위 그림은 **칩 다중 스레딩**으로 알려진 기술이다.
    - 4개의 컴퓨팅 코어가 있고, 각 코어에는 2개의 하드웨어 스레드가 있다.
    - 이를 8개의 논리적 CPU로 볼 수도 있다.
    - Intel 프로세서는 단일 코어에 여러 하드웨어 스레드를 할당하기 위해 **하이퍼-스레딩**을 사용한다고 한다.

<br/>

<br/>

> **처리기를 다중 스레드화하는 2가지 방법**
> 
1. ***거친 다중 스레딩***
    1. 긴 지연시간을 가진 이벤트가 발생할 때까지 한 코어에서 수행된다.
    2. 프로세스 코어에서 다른 스레드가 수행되기 전에 명령어 파이프라인이 정리되어야 한다.
        
        → 스레드 간 교환 비용이 많이 든다.
        
<br/>

<br/>

    
2. ***세밀한 다중 스레딩***
    - 보통 명령어 주기의 경계에서 같이 좀 더 세밀한 정밀도를 가진  시점에서 스레드 교환이 일어난다.
    - 스레드 교환을 위한 회로를 포함한다.
        
        →  스레드 간 교환 비용이 적어진다.
        

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/two-level-scheduling.png" width = 600/> 

- 다중 스레드 다중 코어 프로세서는 위 그림처럼 다른 스케줄링 단계가 필요하다.
    - 1단계 : 소프트웨어 스레드를 선택할 때 결정해야 하는 스케줄링을 결정
    - 2단계 : 각 코어가 실행할 하드웨어 스레드를 결정하는 방법을 명시
        - 명시하는 방법에는 크게 2가지 방법이 있다.
        - 첫번째 방법 : 간단한 RR을 사용해 처리 코어에 하드웨어 스레드를 스케줄
        - 두번째 방법 : 코어당 2개의 하드웨어 스레드를 가진 이중 코어 프로세서를 사용
            
            → 이때 각 하드웨어 스레드는 0~7까지의 동적 **긴급도**가 배정된다.
            
<br/>

<br/>


## 부하 균등화

> SMP에서 처리기가 하나 이상인 것을 활용하려면, **부하를 균등하게 배분하는 것이 매우 중요**하다!
> 

<br/>

<br/>

- ***부하 균등화(load balancing)***이란?
    - SMP에서 모든 처리기 사이에 부하가 고르게 배분되도록 시도하는 것.
    - **push 이주**와 **pull 이주** 방식이 존재한다.

<br/>

<br/>

- push 이주
    - 특정 태스크가 주기적으로 부하를 검사하고, 과부하인 처리기의 부하를 덜 바쁜 처리기로 스레드를 이동시킴!

<br/>

<br/>

- pull 이주
    - 쉬고 있는 처리기가 바쁜 처리기를 기다리고 있는 프로세스를 Pull하는 것을 말한다.

<br/>

<br/>

## 처리기 선호도

> 스레드가 다른 처리기로 이주한다면 캐시 메모리에는 어떤 일이 벌어질까?
> 

1. 첫번째 프로세서의 캐시 메모리의 내용은 무효화되어야 한다.
2. 두번째 프로세서의 캐시는 다시 채워져야 한다.

<br/>

→ 캐시 무효화 및 다시 채우는 비용은 많이 든다!

→ 따라서 스레드를 이주시키지 않고 같은 프로세서에서 계속 실행시키면서 *warm cache*를 이용하려고 한다.

→ 이를 **프로세서 선호도**라고 한다.

<br/>

<br/>

- 처리기 선호도는 여러 형태를 띤다.
    - **약한 선호도** : OS가 동일 처리기에서 프로세서는 실행하려는 정책을 가지고 있지만 보장하지는 않을 때
        - 이 경우, 프로세스가 처리기 사이에서 이주하는 것이 가능하다.
    - **강한 선호도** : 프로세서를 실행하려는 정책을 보장할 때

<br/>

<br/>

- 메인 메모리 아키텍처는 프로세서 선호도 문제에도 영향을 줄 수 있다.
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/NUMA-and-CPU-scheduling.png" width = 600/> 

    - **부하 균등은 종종 프로세서 선호도의 이점을 상쇄한다!**
    - 즉, 동일한 프로세서에서 스레드를 계속 실행 → 해당 프로세서의 캐시를 계속 사용하는 이점
        - 따라서 부하를 균등하게 조정하면 이러한 이점이 사라진다.

<br/>

<br/>

## 이기종 다중 처리

- **이기종 다중 처리**(`heoerogeneous multiprocessings`, HMP)란?
    - 동일한 명령어 집합을 실행하지만, 전력 소비를 유휴 수준으로 저정하는 기능을 포함하여
    - 클록 속도 및 전력 관리 측면에서 차이가 나는 코어를 사용하여 설계된 방식
    - **작업의 특정 요구에 따라 특정 코어에 특정 작업을 할당하여 전력 소비를 더 잘 관리하는 것이 목표**

<br/>

<br/>

- HMP의 장점
    - 백그라운드 작업같은 긴 시간동안 실행해야 하는 작업을 little 코어에 할당해 배터리 충전을 보존한다.
    - 짧은 기간 동안 실행될 수 있는 대화형 응용 프로그램을 big 코어에 할당할 수 있다.

<br/>

<br/>

# 💡 실시간 CPU 스케줄링

- 실시간 시스템은 두 가지로 구분된다.
    1. ***연성 실시간 시스템***
        1. 중요한 실시간 프로세스가 스케줄되는 시점에 아무런 보장을 하지 않음
        2. 즉, 중요 프로세스가 다른 프로세스에 비해 우선권을 가지는 것만 보장
        
    2. ***경성 실시간 시스템***
        1. 더 엄격한 요구 조건을 만족시켜야 한다.
        2. 태스크가 반드시 마감시간까지 서비스를 받아야 한다.

<br/>

<br/>

## 지연시간 최소화

- **이벤트 지연 시간**이란?
    - 실시간 시스템에서 이벤트가 발생했을 때 그에 맞는 서비스가 수행될 때 까지의 시간이다.
    - 크게 ***인터럽트 지연 시간***과 ***디스패치 지연 시간***이 존재한다.

<br/>

<br/>

- **인터럽트 지연시간**
    - CPU에 인터럽트가 발생한 시점부터 해당 인터럽트 처리 루틴이 시작하기까지의 시간
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/interrupt-latency.png" width = 600/>
    
    - 동작 방식
        - 인터럽트 발생 → OS가 수행중인 명령어를 완수하고 발생한 인터럽트 종류를 결정
        - 프로세스 상태를 저장하고, 해당 인터럽트 서비스 루틴(ISR)을 사용해 인터럽트를 처리
    
    <br/>

    - 인터럽트 지연시간에 영향을 주는 요인 중 하나는 **인터럽트 불능 시간**이다. → 매우 짧게 하기!
        - ex) 커널 데이터 구조체를 갱신하는 동안 인터럽트가 불능케 되곤 한다.

<br/>

<br/>

- **디스패치 지연시간**
    - 스케줄링 디스패처가 하나의 프로세스를 블록시키고 다른 프로세스를 시작하는데 까지 걸린 시간
        
        <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/dispatch-latency2.png" width = 600/>
        
    - CPU를 즉시 사용해야 하는 실시간 태스크가 있다면, OS는 이 지연시간을 최소화해야 한다.
    - 따라서 이에 효과적인 방법은 **선점형 커널**이다.
    
    <br/>

    - 디스패치 지연 시간의 충돌 단계는 아래 두 가지 요소로 구성된다.
        1. 커널에서 동작하는 프로세스에 대한 선점
        2. 높은 우선순위의 프로세스가 필요한 자원을 낮은 우선순위 프로세스 자원이 방출

<br/>

<br/>

## 우선순위 기반 스케줄링

> 🌱 실시간 운영체제에서 가장 중요한 기능은 실시간 프로세스에 CPU가 필요할 때 바로 응답하는 것!
>
> 따라서 스케줄러는 선점을 이용한 우선순위 기반의 알고리즘을 지원해야만 한다.


<br/>

<br/>


- 스케줄 될 프로세스들은 **주기적**이다.
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/periodic-task.png" width = 600/>   
    
    1. 각각의 주기 프로세스들은 고정된 수행시간(t)과 마감시간(d), 주기(p)가 존재한다.
    2. 이때, 주기 태스크의 **실행 빈도**는 1/p 이다.
    
    <br/>

    **→ 스케줄러는 마감 시간과 프로세스의 실행 빈도에 따라 우선순위를 정한다.**
    

<br/>

<br/>

- 이러한 형식의 특징!
    - 프로세스가 자신의 마감시간을 스케줄러에게 알려야만 한다.
    - 따라서 **승인 제어**가 필요!
        
        → 스케줄러가 마감시간 이내에 완수할 수 있는 프로세스만 실행을 허락함.
        

<br/>

<br/>

## Rate-Monotonic 스케줄링

- **Rate-Monotonic 스케줄링**이란?
    - **선점 가능한 정적 우선순위 정책**을 이용하여 주기 태스크들을 스케줄한다.

<br/>

<br/>

- 단순히 정적 우선순위 정책을 사용할 경우
    - P1는 마감시간을 충족하지 못한다.
        
        
        <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/rate-monotonic1.png" width = 600/>
        

<br/>

<br/>

- **Rate-Monotonic 스케줄링**
    
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/rate-monotonic2.png" width = 600/>
    

<br/>

<br/>

## Earliest-Deadline-First 스케줄링

- **Earliest-Deadline-First(EDF) 스케줄링**이란?
    - 마감시간에 따라 우선순위를 동적으로 부여한다.
    

<br/>

<br/>

> EDF에서는 프로세스가 실행 가능하게 되면 자신의 마감시간을 시스템에게 알려야 한다!
> 

<br/>

<br/>

- 우선순위는 새로 실행 가능하게 된 프로세스의 마감시간에 맞춰 다시 조정된다.
    - **이 점이 우선순위가 고정되어 있는 rate-monotonic 스케줄링과 다른 점이다.**

<br/>

<br/>

- 동작 설명
    
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/EDF.png" width = 600/>
    

<br/>

<br/>

## 일정 비율의 몫 스케줄링

- **일정 비율의 몫(proportional share)** 스케줄러란?
    - 모든 응용들에 T개의 시간 몫을 할당하여 동작한다.
    - ex) N개의 시간 몫을 할당 받으면, 그 응용은 N/T 시간을 할당받는다.

<br/>

<br/>

> 일정 비율의 몫 스케줄러는 시간 몫을 할당받는 것을 보장하는 **승인 제어 정책과 함께 동작**해야만 한다.
> 

<br/>

<br/>

## POSIX 실시간 스케줄링

- POSIX는 실시간 스레드를 위해 두 개의 스케줄링 클래스를 정의한다.
    - SCHED FIFO
        - FIFO 큐를 이용해 스레드를 스케줄한다.
        
    <br/>

    - SCHED RR
        - 라운드 로빈 정책을 이용해 같은 우선순위의 경우, 시간 할당량을 제공한다.

<br/>

<br/>

- POSIX API는 스케줄링 정책에 관한 정보를 지정하고 얻어내는 함수를 제공한다.
    - `pthread attr getschedpolicy(pthread attr t *attr, int *policy)`
    - `pthread attr setschedpolicy(pthread attr t *attr, int policy)`
    - 첫번째 인자는 스레드의 속성 집합에 대한 포인터이고,
    - 두번째 인자는 현재 스케줄링 정책이거나 정책을 표현하는 정수이다.


<br/>

<br/>

# 💡 운영체제 사례들

> Linux, Window 및 Solaris 운영체제의 스케줄링 정책을 알아보자.
>
>여기선 ***프로세스 스케줄링***을 일반적인 의미로 사용함을 인지하자!
>
>Solaris, Windows는 ***커널 스레드*** 스케줄링에 관해 설명하고, Linux는 ***태스크 스케줄링***에 관해 말한다.
> 

<br/>

<br/>

## Linux 스케줄링

- **Version 2.5 이전**
    - 전통적인 UNIX 스케줄링 알고리즘의 변형을 사용!
    - **But**, 이는 SMP를 염두하지 않아 다중 처리기 시스템을 충분히 지원하지 않음.

<br/>

<br/>

- **Version 2.5**
    - O(1) 스케줄링 알고리즘을 포함
        
        → 스케줄러가 철저하게 분석되어 시스템에 존재하는 태스크 개수와 상관없이 항상 상수 시간에 실행됨
        
        → 이 스케줄러는 또한 SMP를 위한 향상된 지원을 제공한다.
        
    - SMP 시스템에서 출중한 성능을 보이지만, 데스크톱의 대화형 프로세스에 대해서는 느린 응답을 가짐

<br/>

<br/>

- **Version 2.6**
    - 2.6.23 커널부터는 **완전 공평 스케줄러(CFS)**가 Linux 시스템의 디폴트 스케줄링 알고리즘이 됐다!

<br/>

<br/>

- Linux 스케줄러는 각 클래스별로 특정 우선순위를 부여받는 **스케줄링 클래스**에 기반을 두고 동작한다.
    - *요구 조건에 따라 다른 스케줄링 클래스를 사용 가능하다!*

<br/>

<br/>

> CFS 스케줄러의 특징
> 
- 각 태스크에 CPU 처리시간의 비율을 할당한다.
    - 이 비율은 각 태스크에 할당된 **nice 값**에 기반을 두고 계산된다.
    - nice 값의 범위는 -20부터 19까지이며, 값이 적을수록 우선순위가 높다.
- 이산 값을 가지는 시간 할당량을 사용하지 않고 **목적 지연시간**을 찾는다.
    - *목적 지연시간이란, 다른 모든 수행 가능한 태스크가 최소 한번은 실행할 수 있는 시간 간격이다.*
    - CPU 시간의 비율은 이 목적 지연시간의 값으로부터 할당된다.
- 직접 우선순위를 할당하지 않는다.
    - 각 태스크별로 vruntime이라는 변수에 실행된 시간을 기록해 **가상 실행 시간**을 유지한다.
    - CFS는 이 vruntime 값을 균형이진 탐색 트리인 적-백 트리 형태로 관리한다.
        
        
        <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/vruntime.png" width = 600/>    
        

<br/>

<br/>

- ***CFS 스케줄러 동작 검토하기***
    - nice 값이 같은 두 태스크가 존재한다고 가정.
        - 하나는 I/O 중심 태스크, 다른 하나는 CPU 중심 태스크이다.
        - I/O 중심 → 짧은 시간 실행 후 봉쇄 / CPU 중심 → 최대한 시간 주기를 소비하려고 노력
    - 따라서 vruntime 값 : CPU 중심 > I/O 중심 이므로 우선순위는 I/O 중심태스크가 커진다.
    - 결론은 I/O 중심 태스크가 실행 가능해지면, 현재 실행중인 CPU 중심 태스크를 선점한다!

<br/>

<br/>

- 이전 절에서 설명했듯이, Linux는 POSIX 표준을 사용해 실시간 스케줄링을 구현한다.
    - `SCHED_FIFO` 또는 `SCHED_RR` 실시간 정책을 사용하고, 스케줄 된 태스크는 높은 우선순위를 가진다
    - Linux에는 우선순위 영역이 아래와 같이 크게 두 가지로 나뉜다.
        
        
        <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/Linux-scheduling-priority.png" width = 600/>
        

<br/>

<br/>

> CSF 스케줄러는 부하 균등화를 지원한다.
> 
- 이때, 처리 코어 간의 부하를 균등하게 유지한다.
- 동시에 NUMA를 인식해 스레드 이주를 최소화하는 정교한 기술을 사용한다.

<br/>

<br/>

>🌱 CFS는 무엇을 기준으로 부하를 정의할까?
>
>1. 스레드 우선순위
>2. 평균 CPU 이용률


<br/>

<br/>

- Linux는 NUMA에서 스레드 이주의 단점을 보완하기 위해 **스케줄링 도메인**의 계층적 시스템을 결정한다!
    - 스케줄링 도메인이란 서로 부하의 균형을 맞출 수 있는 CPU 코어의 집합이다.
        
        
        <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/NUMA-LinuxCFS.png" width = 600/>
        
    
    <br/>

    - 기본 구성
        - 코어는 자신만의 캐시를 가진다. → L1 캐시
        - 한 쌍의 코어는 L2 캐시를 공유한다. → 도메인 생성
        - 두 도메인은 L3 캐시를 공유한다. → 프로세서 수준 도메인(**NUMA 노드**)
    
    <br/>

    - 동작 방식
        - CFS의 기본 전략은 가장 낮은 수준의 층부터 부하의 균형을 맞춘다.
        - 처음에는 스레드가 동일한 도메인 간에만 이주된다.
        - 다음은 도메인 사이에서 부하 균등화가 일어난다.

<br/>

<br/>

## Windows 스케줄링

> Windows는 우선순위에 기반을 둔 선점 스케줄링 알고리즘을 사용한다.
> 
- 따라서 스케줄러는 가장 높은 우선순위의 스레드가 항상 실행되도록 보장한다.
- 스케줄링을 담당하는 **디스패처**는 실행 순서를 정하기 위해 32단계의 우선순위를 가진다.

<br/>

<br/>

> 디스패처의 우선순위는 두 클래스로 구분된다.
> 
- 가변 클래스
    - 우선순위가 1~15까지다.
- 실시간 클래스
    - 우선순위가 16~31까지다.
- 이때, 우선순위가 0인 스레드는 메모리 관리를 위해 사용된다.
- 각 큐에서 준비 상태의 스레드가 없으면 디스패처는 **idle 스레드**를 실행시킨다.

<br/>

<br/>

> Windows API는 프로세스들이 속하는 6가지의 우선순위 클래스를 제공한다.
→ 우선순위는 `SetPriorityClass()` 를 통해 변경될 수 있다.
> 
- `IDLE PRIORITY CLASS`
- `BELOW NORMAL PRIORITY CLASS`
- `NORMAL PRIORITY CLASS`
    - 통상 대부분의 프로세스들이 속한다.
- `ABOVE NORMAL PRIORITY CLASS`
- `HIGH PRIORITY CLASS`
- `REALTIME PRIORITY CLASS`
    - 유일하게 위 클래스들과 다르게 가변적이지 않다.

<br/>

<br/>

> 우선순위 클래스의 스레드들 역시 상대적인 우선순위를 가진다.
> 
- `IDLE`
- `ABOVE NORMAL`
- `LOWEST`
- `HIGHEST`
- `BELOW NORMAL`
- `TIME CRITICAL`
- `NORMAL`

<br/>

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/windows-thread-priority.png" width = 600/>

- 스레드의 우선순위는 그 스레드가 속한 우선순위 클래스와 그 클래스 내의 상대적 우선순위에 기반을 둔다.
    - 또한 각 우선순위 클래스에 주어어진 기본 우선순위는 normal 행과 같다!
    - ex) 24, 13, 10, 8, 6, 4

<br/>

<br/>

> Windows는 대화형 프로그램이 실행 중일 때 해당 프로세스가 특별히 좋은 성능을 얻도록 한다.
> 
- 이를 위해 `NORMAL PRIORITY CLASS` 에 있는 프로세스에 특별한 스케줄링 법칙을 적용한다.
    1. 현재 화면에 선택된 **포그라운드 프로세스**와 선택되지 않은 **백그라운드 프로세스**를 구분한다.
    2. 포그라운드 프로세스가 실행되면 시간 할당량을 3배 정도 증가시켜 오래 수행될 수 있도록 한다.

<br/>

<br/>

- Windows 7은 **사용자 모드 스케줄링(UMS)**을 도입하였다.
    - 이는 응용 프로그램이 커널과는 독립적으로 스레드를 생성, 관리할 수 있게 한다.
    - 따라서 사용자 모드에서 스레드를 스케줄하므로 커널의 개입이 없어 훨씬 효율적이다.

<br/>

<br/>

- Windows의 초기 버전들은 **fiber**라고 불렸던 유사한 기능을 제공한다.
    - fiber → 많은 사용자 스레드가 하나의 커널 스레드에 사상되는 것이 가능하도록 한다.
    - UMS는 모든 사용자 스레드가 각자의 문맥을 저장할 수 있게 하여 기존의 fiber의 제약을 극복했다.

<br/>

<br/>

## Solaris 스케줄링

> Solaris는 우선순위 기반 스레드 스케줄링을 사용한다.
> 
- 우선순위에 따라 아래와 같이 6개의 스케줄링 클래스를 정의한다.
    - 시분할(`time-sharing`, TS)
    - 대화형(`interactive`, IA)
    - 실시간(`real time`, RT)
    - 시스템(`system`, SYS)
    - 공평 공유(`fair share`, FSS)
    - 고정 우선순위(`fixed priority`, FP)

<br/>

<br/>

- 시분할 클래스
    - 프로세스의 디폴트 스케줄링 클래스
    - 다단계 피드백 큐를 사용하여 동적으로 우선순위를 바꾸고, 서로 다른 타임 슬라이스를 할당

<br/>

<br/>

- 대화형 클래스
    - 시분할 클래스와 같은 정책을 사용하지만, 더 나은 성능을 위해 KED 또는 GNOME 윈도 관리자에 의해 생성된 윈도윙 응용에 더 높은 우선순위를 준다.

<br/>

<br/>

- 실시간 클래스
    - 이 클래스의 스레드에는 가장 높은 우선순위가 주어진다.
    - 따라서 제한된 주기 내에 시스템으로부터 응답을 보장받도록 한다.

<br/>

<br/>

- 시스템 클래스
    - Solaris가 커널 스레드를 실행하기 위해 사용한다.
    - 시스템 스레드의 우선순위는 한 번 설정되면 바뀌지 않는다.

<br/>

<br/>

- 고정 우선순위 클래스
    - 이 클래스의 스레드는 시분할 클래스와 같은 우선순위를 갖는다.
    - 단, 우선순위가 동적으로 조정되지 않는다!
    
<br/>

<br/>

- 공평 공유 클래스
    - 스케줄링 결정을 위해 우선순위 대신에 CPU **공유량(share)**을 사용한다.
    - CPU 공유량은 가용한 CPU 자원에 대한 권리를 가리키고, **프로젝트**라고 불리는 집합에 할당된다.

<br/>

<br/>

# 💡 알고리즘 평가

## 결정론적 모델링

- 평가 방법의 한 중요한 부류로 **분석적 평가(analytic evaluation)**가 있다.
    - 분석적 평가에서는 알고리즘의 성능을 평가하는 공식이나, 시스템 작업 부하를 이용한다.
    - 분석적 평가의 한 가지 유형은 **결정론적 모델링**이다.
        - ***이 방식은 사전에 정의된 특정 작업 부하를 받아들여, 그에 대한 각 알고리즘의 성능을 정의한다.***

<br/>

<br/>

- 동작 방식

    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/dicision.png" width = 600/>
    
    - 프로세스들이 시간 0에 위와 같이 도착했다고 가정하자.
    - 결정론적 모델링은 위 프로세스들을 각 스케줄링(FCFS, SJF, RR)을 한 후 결과를 비교하도록 한다.
    - ***따라서 단순하고 빠르지만, 입력으로 정확한 숫자를 요구한다는 단점이 있다.***

<br/>

<br/>

## 큐잉 모델

- 컴퓨터 시스템은 서버들의 네트워크로 기술되고, 각 서버는 대기 프로세스들의 큐를 가진다.
    - 우리는 이용률, 평균 큐 길이, 평균대기 시간 등을 계산하는 **큐잉 네트워크 분석**을 할 수 있다.
    - 예시
        
        > n : 평균 큐 길이, W = 평균대기 시간, ㅅ = 새로운 프로세스의 평균 도착률일 때, 아래 식을 만족
        > 
        
        $n = ㅅ * W.$ → 이를 **Little’s formula**라고 한다.
        

<br/>

<br/>

- 큐잉 분석은 스케줄링 알고리즘을 비교하는데에 유용하다!
    - 단, 현재 알고리즘과 분포의 부류가 상당히 제한적이고, 복잡한 알고리즘이나 수학적 분석은 어렵다.
    - 또한 정확하지 않을 수 있는, 다수의 독립된 가정을 하는 것이 일반적이다.

<br/>

<br/>

## 모의 실험

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/simulation.png" width = 600/>

- 스케줄링 알고리즘을 더욱 정확하게 평가하기 위한 방법으로, 시스템 모델을 프로그래밍하는 것을 포함한다
    - 모의실험기는 시간을 나타내는 변수를 갖고, 이 변수 값을 증가시키며 결과에 반영한다.
    - 모의실험을 위한 자료들은 보편적으로 난수 발생기를 이용한다.

<br/>

<br/>

- 다만, 분포에 의해 주도되는 모의실험은 실제 시스템에서는 부정확할 수 있다.
    - 이를 해결하기 위해 **추적 파일**을 사용하자.
        - 추적파일은 실제 시스템을 관찰해 실제 이벤트들의 순서를 기록하여 만든 파일이다.
- 또한 모의실험은 컴퓨터를 오래 사용하여 큰 비용이 소요될 수 있다.

<br/>

<br/>

## 구현

- 스케줄링 알고리즘을 정확하게 평가하는 유일한 방법은 실제 코드로 운영체제에서 실행시켜보는 것이다.
    - 이 방식은 비용이 많이 든다. (가상 머신에서 변경 사항을 테스트하는 데도 마찬가지다.)
    - 다만, **회귀 테스트**를 통해 변경사항의 성능 체크와 새로운 버그 체크 등을 확인할 수는 있다.

<br/>

<br/>

- 일반적으로 가장 융통성있는 스케줄링 알고리즘
    - 시스템 관리자 또는, 사용자에 의해 알고리즘이 특정 응용에 맞게 조정될 수 있도록 하는 것이다.
    - 일부 OS는 시스템 관리자가 특정 시스템 구성을 위해 스케줄링 변수들을 미세 조정할 수 있게도 한다.

<br/>

<br/>

- 다른 접근 방법
    - 프로세스나 스레드의 우선순위를 변경하는 API를 사용하자.
        - 이는 Java, POSIX 및 WinAPI가 제공한다.
    - 이 방식의 단점은 더 일반적인 상황에서는 개선된 성능을 낳지 않을 수 있다는 것이다.