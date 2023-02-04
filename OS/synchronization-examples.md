# 동기화 예제
## 💡 고전적인 동기화 문제들

### 유한 버퍼 문제

> 이 문제는 일반적으로 동기화 프리미티브들의 능력을 설명하기 위해 사용된다.
어느 특정 구현에 국한됨 없이 이 해결 방법의 일반적인 구조를 제시한다.
> 

<br/>

<br/>

- 소비자 - 생산자가 아래와 같은 자료구조를 공유한다고 가정.
    
    ```java
    int n;
    semaphore mutex = 1;
    semaphore empty = n;
    semaphore full = 0;
    ```
    
    - 내용 설명
        - n개의 버퍼로 구성된 풀이 있고, 각 버퍼는 한 항목을 저장할 수 있다.
        - mutex 이진 세마포는 버퍼 풀에 접근하기 위해 상호 배제 기능을 제공하며, 1로 초기화
            - 따라서 `empty`와 `full`로 기록한다.

<br/>

<br/>

- 생산자 코드
    
    ```java
    while (true) {
    		/* produce an item 
    		in next_produced */
    		...
    		wait(empty);
    		wait(mutex);
    		...
    
    		/* add next_produced 
    		to the buffer */
    		...
    		signal(mutex);
    		signal(full);
    }
    ```
    
<br/>

<br/>

- 소비자 코드
    
    ```java
    while (true) {
    		wait(full);
    		wait(mutex);
    		...		
    		/* remove an item from 
    		buffer to next_consumed */
    		
    		...
    		signal(mutex);
    		signal(empty);
    		...
    		/* consume the item 
    		in next_consumed */
    }
    ```
    
<br/>

<br/>


### Reader-Writers 문제

> `readers-writers` 문제란?
> 
> - writer가 쓰기 작업 동안에 공유 데이터베이스에 대해 배타적 접근 권한을 가지게 할 필요가 있다.
> - 이때, 이 동기화 문제를 readers-writers 문제라고 한다.

<br/>

<br/>

> `readers-writers` 문제에는 여러 가지 변형들이 있다.
> 
> 1. Writer가 공유 객체를 사용할 수 있는 허가를 얻지 못했다면, 어느 Reader도 기다리게 해서는 안된다.
> 2. Writer가 준비되면 가능한 한 빨리 쓰기를 수행할 것을 요구한다.

<br/>

<br/>

- 우리는 위 문제에 대하 해결안이 기아 상태를 낳을 수 있음을 유의해야 한다.
    - 첫 번째 경우에는 Writer가 기아 상태에 빠질 수 있고,
    - 두 번째 경우에는 Reader가 기아 상태에 빠질 수 있다.

<br/>

<br/>

- 첫 번째 Readers-Writers 문제에 대한 해결안
    - Reader 프로세스는 아래의 자료구조를 공유한다고 가정.
        
        ```c
        semaphore rw_mutex = 1;
        semaphore mutex = 1;
        int read_count = 0;
        ```
        
        - `mutex` 세마포 : read_count를 갱신할 때 상호 배제를 보장하기 위함
        - `rw_mutex` : writer들을 위한 상호 배제 세마포

<br/>

<br/>

- Writer 프로세스의 구조
    
    ```c
    while (true) {
    		wait(rw_mutex);
    		...
    		/* writing is performed */
    		...
    		signal(rw_metex);
    }
    ```
    
<br/>

<br/>

- Reader 프로세스의 구조
    
    ```c
    while (true) {
    		wait(mutex);
    		read_count++;
    		if (read_count == 1)
    				wait(rw_mutex);
    		signal(mutex);
    
    		...
    		/* reading is performed */
    		...
    		wait(mutex);
    		read_count--;
    		if (read_count == 0)
    				signal(rw_mutex);
    		signal(mutex);
    }
    ```
    

<br/>

<br/>

> `Readers-writers` 문제와 그의 해결안들은 일반화되어 **read-writer 락**을 제공하는 시스템들이 있다.
> 
> - 락을 획득할 때에는 읽기인지 쓰기인지의 모드를 지정해야만 한다.
> - 프로세스가 공유 데이터를 읽기만 원한다면?
>     - 읽기 모드의 락을 요청한다.

<br/>

<br/>

> `Readers-writer` 락은 다음과 같은 상황에서 가장 유용하다.
> 
> - 공유 데이터를 읽기만 하는 프로세스와 쓰기만 하는 스레드를 식별하기 쉬운 응용
> - Writer보다 reader의 개수가 많은 응용.

<br/>

<br/>

### 식사하는 철학자들 문제

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/dining-philosophers.png" width = 600/>

<br/>

<br/>

> 식사하는 철학자들 문제는 **고전적인 동기화 문제**로 간주된다.
많은 문제의 한 예시로, 교착 상태와 기아를 발생시키지 않고 여러 스레드에게 여러 자원을 할당함을 의미한다.
> 

<br/>

<br/>

### 세마포 해결안

> 한가지 간단한 해결안 : 각 젓가락을 하나의 세마포로 표현!
> 
> - 철학자는 그 세마포에 wait() 연산을 실행해 젓가락을 집으려 시도한다.
> - 또한 해당 세마포에 signal() 연산을 실행해 자신의 젓가락을 놓는다.

<br/>

<br/>

- 위 가정을 위한 공유 자료는 아래와 같다.
    
    ```c
    semaphore chopstick[5];
    ```
    

<br/>

<br/>

- 철학자 `i`의 구조
    
    ```c
    while (true) {
    		wait(chopstick[i]);
    		wait(chopstick[(i + 1) % 5]);
    		...
    		/* eat for a while */
    		...
    		signal(chopstick[i]);
    		signal(chopstick[(i + 1) % 5]);
    		...
    		/* think for awhile */
    		...
    }
    ```
    

<br/>

<br/>

> 이 해결안은 인접한 두 철학자가 동시에 식사하지 않는다는 것을 보장하지만, 교착 상태를 야기할 수 있다.
> 
> - ex) 모든 철학자가 동시에 배가 고파 각각 자신의 왼쪽 젓가락을 잡는다고 가정!
> - 이때, 각 철학자가 오른쪽 젓가락을 집으려고 하면 영원히 기다려야 한다.

<br/>

<br/>

> 교착 상태 문제에 대한 여러 가지 해결책
> 
> - 최대 4명의 철학자만이 테이블에 동시에 앉을 수 있도록 한다.
> - 한 철학자가 젓가락 두 개를 모두 집을 수 있을 때만 젓가락을 집도록 허용한다.
> - 비대칭 해결안을 사용한다.

<br/>

<br/>

### 모니터 해결안

> 이 해결안은 철학자가 양쪽 젓가락을 모두 얻을 수 있을 때만 젓가락을 집을 수 있다는 제한을 강제한다.
> 
> - 따라서 교착 상태가 없는 해결안이다.

<br/>

<br/>

- 이 해결안을 구현하기 위해 철학자의 상태를 구분한다.
    
    ```c
    enum {THINKING, HUNGRY, EATING} state[5]; 
    ```
    
    - 양쪽 두 이웃이 식사하지 않을때만 `state[i] = EATING` 으로 설정할 수 있다.

<br/>

<br/>

- 또한 다음을 선언해야 한다.
    
    ```c
    condition self[5];
    ```
    
    - `self`는 철학자 i가 배고프지만 자신이 원하는 집가락을 집을 수 없다면 집기를 미루도록 한다.

<br/>

<br/>

- 모니터 해결안 코드
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/monitor-solution.png" width = 600/>


<br/>

<br/>

## 💡 커널 안에서의 동기화 

### Windows의 동기화

> Windows는 커널 외부에서 스레드를 동기화하기 위해 **dispatcher** 객체를 제공한다.
> 
> - 스레드는 이 객체를 사용해 mutex락, 세마포, event 등을 포함한 다양한 기법에 맞추어 동기화한다.
> - Event는 조건 변수와 유사하다! → 기다리는 조건이 만족하면 스레드에 통지하는 역할

<br/>

<br/>

- Dispatcher 객체는 `signaled` 상태 혹은 `nonsignaled` 상태이다.
    - `signaled` : 객체가 사용 가능하고 그 객체를 얻을 때 그 스레드가 봉쇄되지 않음.
    - `nonsignaled` : 객체가 사용할 수 없고, 그 객체를 얻으려고 시도하면 스레드가 봉쇄됨.

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/mutex-dispatcher.png" width = 600/>

<br/>

<br/>

> Dispatcher 객체의 상태와 스레드 상태 간에는 관련성이 있다.
> 
> - 스레드가 `nonsignaled` 상태에 있는 dispatcher 객체 때문에 봉쇄되면 그 스레드의 상태는 준비로부터 대기 상태로 바뀌고 그 스레드는 그 객체의 대기 큐에 넣어지게 된다.
> - 추후 dispatcher 객체의 상태가 `signaled` 상태로 바뀌면 커널은 그 객체를 기다리는 스레드가 있는지 여부를 알아낸다.

<br/>

<br/>

- **Critical-section** 객체란?
    - 커널의 개입 없이 획득하거나 방출할 수 있는 사용자 모드 mutex이다.
    - `critical-section` 객체는 커널 mutex가 객체에 대한 경쟁이 발생할 때만 할당되기 때문에 효율적이다! → 실제로 CPU 절약이 상당히 좋아진다!

<br/>

<br/>

### Linux의 동기화

> Linux는 커널 안에서 동기화할 수 있는 많은 다른 기법을 제공한다.
> 
> - 가장 간단한 기법은 원자적 정수를 이용하는 것이다.

<br/>

<br/>

- 아래 코드는 다양한 원자적 연산을 수행한 효과이다.
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/atomic-operation.png" width = 600/>
    

<br/>

<br/>

- 원자적 정수는 counter(정수형 변수)가 갱신되어야 하는 상황에서 특히 효율적이다!
    - 왜냐면 원자적 연산을 락 기법을 사용할 때는 오버헤드가 필요하지 않기 때문!

<br/>

<br/>

- Linux 커널은 커널 안에서의 락킹을 위해 스핀락과 세마포 및 두 락의 reader-writer 버전도 제공한다.
    - SMP 기계에서는 기본적인 락킹 기법은 스핀락이다.
    - 간단하게 요약하면 아래와 같다.
        
        <br/>

        <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/Linux-smp.png" width = 600/>
        
        - Linux 커널에서 스핀락과 mutex 락은 ***재귀적이지 않다!***
            - 즉, 스레드가 이러한 락 중 하나를 획득했다면?
            - → 락을 해제하지 않고는 같은 락을 다시 획득할 수 없다.


<br/>

<br/>
