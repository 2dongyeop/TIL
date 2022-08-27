## CPU 스케줄링

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-scheduling1.png" width = 500/>
### 스케줄링 알고리즘이란? 
-  **준비 큐**에 있는 프로세스 중 **어느 프로세스에게 CPU를 할당할 것인지 결정**하는 문제이다.

<br/>

### CPU 스케줄링 알고리즘 비교 기준
- CPU 이용률 (utilization)
- 처리량 (throughput)
	- 단위 시간당 완료된 프로세스의 개수
- 총처리 시간 (turnaround time)
	- 프로세스의 제출 시간과 완료 시간의 간격
	- 준비 큐에서 대기한 시간 + CPU에서 실행한 시간 + 입출력 시간
- **대기 시간 (waiting time)**
	- 준비 큐에서 대기하는 시간
	- CPU 스케줄링 알고리즘은 프로세스 실행 시간과 입출력 시간에는 영향을 미치지 않는다
- 응답 시간 (response time)
    - 요구를 제출하고부터 첫 번째 응답이 나올 때까지의 시간

<br/>

### 선점 스케줄링

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-scheduling2.png" width = 500/>

- 낮은 우선순위를 가진 프로세스보다 높은 우선순위를 가진 프로세스가 CPU를 선점하는 방식
- OS가 스케줄링의 알고리즘에 따라 적당한 프로세스에게 CPU를 할당하고, **필요시에는 회수**하는 방식

<br/>

### 비선점 스케줄링

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-scheduling3.png" width = 500/>
- Time-slice 가 없는 스케줄링
- CPU를 사용중인 프로세스가 **자율적으로 반납**하도록 하는 방식
- 따라서 프로세스가 자율적으로 CPU를 반납하는 시점에서 사용한다.


<br/>

### 선입선출 스케줄링 (FCFS - First Come First Served)

- **먼저 CPU를 요청하는 프로세스를 먼저 처리**하는 방식
- **선입선출(FIFO) 큐**로 구현
- 단점으로는 평균 대기 시간이 긴 경우가 있다.

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-scheduling4.png" width = 500/>

- 특징으로는 **비선점형** 스케줄링이다.
- **호위 효과**(convoy effect)가 발생 : short process behind long process
	- 이는 짧은 I/O 중심의 프로세스들이 **하나의 긴 CPU 중심 프로세스가 CPU를 양도하기를 기다리는 것**이다.

<br/>

>
### 최단 작업 우선 스케줄링 (SJF - Short Job First)


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-scheduling5.png" width = 500/>
- **평균 대기 시간을 최소화**하기 위해 사용하는 방식
- **버스트 시간이 짧은 프로세스부터 CPU를 할당**한다.
- 문제는 다음 CPU 버스트의 길이를 알 수 없어, 이상적인 설계라고만 할 수 있다.
- 특징으로는 선점형/비선점형 방식이 존재한다.
  -	 비선점형 : 실행중인 프로세스가 CPU 버스트를 완료할 때까지 선점하지 않는다.
  - 선점형 : 새로운 프로세스가 **현재 실행되고 있는 프로세스의 남은 시간보다 짧은 버스트를 가지면** 현재 실행중인 프로세스를 선점한다.


> 선점형 최단 작업 우선 스케줄링 = 최소 잔여 시간 우선 스케줄링 
> 
> (SRTF : Shortest Remaining Time First)


<br/>


### 라운드 로빈 스케줄링 (RR - Round Robin)


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-scheduling6.png" width = 500/>
- **시분할 시스템**을 위해 설계된 스케줄링
- 모든 프로세스가 같은 우선순위를 가지고, **시간 할당량을 기반**으로 스케줄링한다.
- Time slice burst가 일어나면 해당 프로세스는 **스케줄링 큐의 끝으로 이동**한다
- 특징으로는 선점형 스케줄링이다.
- 라운드 로빈 스케줄링의 성능은 시간 할당량의 크기에 매우 많은 영향을 받는다.
  - 시간 할당량이 매우 크면, 선입 선출과 같고,
  - 시간 할당량이 매우 크면, 불필요한 문맥 교환을 야기한다.

<br/>


### 우선 순위 스케줄링 (Priority Scheduling)


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/CPU-scheduling7.png" width = 500/>
- 각 프로세스에게 우선 순위가 주어지고, **우선 순위가 높은 순**으로 CPU가 할당된다.
- **우선 순위가 같은 프로세스들은 선입 선처리 순서로 스케줄링한다.**
- 선점형/비선점형이 있다.
  - 선점형 : 새로 도착한 프로세스의 우선순위가 현재 실행되는 프로세스의 우선순위보다 높으면 선점한다.
  - 비선점형 : 단순히 준비 큐의 머리 부분에 새로운 프로세스를 넣는다.

- **무한 봉쇄(indefinite blocking)**
  - 높은 우선순위의 프로세스가 꾸준히 들어와, 낮은 우선순위의 프로세스가 계속 순서가 밀리는 현상
  - 이 경우 에이징(aging)을 통해 해결할 수 있다.
    - 여기서 에이징이란, **오래 대기하는 프로세스들의 우선순위를 점진적으로 올리는 증가**시키는 것



