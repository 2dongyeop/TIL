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

## 💡 POSIX 동기화 

### POSIX mutex 락

> Pthreads에서 사용할 수 있는 기본적인 동기화 기법을 대표한다.
> 
> - 이는 코드의 임계구역을 보호하기 위해 사용된다.
> - 즉, 스레드는 임계구역에 진입하기 전에 락을 획득하고 임계구역에서 나갈 때 락을 방출한다.

<br/>

<br/>

- Mutex 코드 예시 일부
    
    ```c
    #include <pthread.h>
    pthread mutex t mutex;
    
    /* create and initialize the mutex lock */
    pthread mutex init(&mutex,NULL);
    
    /* acquire the mutex lock */ 
    pthread mutex lock(&mutex);
    
    /* critical section */
    
    /* release the mutex lock */
    pthread mutex unlock(&mutex);
    ```
    

<br/>

<br/>

### POSIX 세마포

> POSIX는 기명(`named`)과 무명(`un-named`)의 두 유형의 세마포를 명기하고 있다.
> 
> - 기본적으로 이 두가지는 매우 유사하지만 프로세스 간에 생성 및 공유되는 방식이 다르다.

<br/>

<br/>

- 기명 세마포 코드
    - `sem_open()` : POSIX 기명 세마포를 생성하고 여는데 사용
    
    ```c
    #include <semaphore.h>
    sem t *sem;
    
    /* Create the semaphore and initialize it to 1 */
    sem = sem open("SEM", O CREAT, 0666, 1);
    
    /* acquire the semaphore */ 
    sem wait(sem);
    
    /* critical section */
    
    /* release the semaphore */
    sem post(sem);
    ```
    
    - ***장점 : 여러 관련 없는 프로세스가 세마포 이름만 참조하여 동기화 기법으로 쉽게 사용 가능하다!***

<br/>

<br/>

- 무명 세마포 코드
    
    ```c
    #include <semaphore.h> 
    sem t sem;
    
    /* Create the semaphore and initialize it to 1 */ 
    sem init(&sem, 0, 1);
    
    /* acquire the semaphore */ 
    sem wait(&sem);
    
    /* critical section */
    
    /* release the semaphore */ 
    sem post(&sem);
    ```
    

<br/>

<br/>

### POSIX 조건 변수

> Pthread의 조건 변수는 pthread_cond_t 데이터 유형을 사용하고, pthread_cond_init()을 사용해 초기화한다.
> 

- 코드
    
    ```c
    pthread mutex t mutex; 
    pthread cond t cond var;
    
    pthread mutex init(&mutex,NULL); 
    pthread cond init(&cond var,NULL);
    
    pthread mutex lock(&mutex); 
    while (a != b)
    		pthread cond wait(&cond var, &mutex); 
    
    pthread mutex unlock(&mutex);
    ```


<br/>

<br/>

## 💡 Java에서의 동기화 

### Java 모니터

> Java는 스레드 동기화를 위한 모니터와 같은 병생성 기법을 제공한다.
> 
> - `BoundedBuffer` 클래스는 생산자와 소비자 문제의 해결안을 구현한다.

<br/>

<br/>

- `BoundedBuffer` 클래스
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/BoundedBuffer.png" width = 600/>
    

<br/>

<br/>

- `synchronized` 메소드를 호출하려면?
    - → BoundedBuffer의 객체 인스턴스와 연결된 락을 소유해야 한다.

<br/>

<br/>

- 다른 스레드가 이미 락을 소유했다면?
    - → 객체의 락에 설정된 **진입 집합**(entry set)에 추가된다.
    - 이 진입 집합은 락이 가용해지기를 기다리는 스레드 집합이다.
        
        <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/entry-set.png" width = 600/>
        

<br/>

<br/>

> 락을 갖는 것 외에도 모든 객체는 스레드 집합으로 구성된 **대기 집합**과 연결됨을 알고 있자!
> 

<br/>

<br/>

- 전체 코드
    
    ```java
    /* Producers call this method */
    public synchronized void insert(E item) {
    		while (count == BUFFER SIZE) { 
    				try {
    						wait(); //버퍼가 가득 차면 호출
    				}catch (InterruptedException ie) {}
    		}
    
    		buffer[in] = item;
    		in = (in + 1) % BUFFER SIZE; 
    		count+;
    		notify();
    }
    
    <br/>

    /* Consumers call this method */ 
    public synchronized E remove() {
    		E item;
    		while (count == 0) { 
    				try {
    						wait();
    				}
    		}catch (InterruptedException
    
    		item = buffer[out];
    		out = (out + 1) % BUFFER SIZE; 
    		count--;
    
    		notify();
    		return item;
    }
    ```
    

<br/>

<br/>

### 재진입 락

> API에서 사용 가능한 가장 간단한 락 기법은 **ReentrantLock**이다.
> 
> - 이는 여러가지 면에서 synchronized 명령문처럼 작동한다.
> - 단일 스레드가 소유하며, 공유 자원에 대한 상호 배타적 액세스를 제공하는 데 사용된다.

<br/>

<br/>

- `ReentrantLock`은 ***공정성*** 매개변수 설정과 같은 몇 가지 추가 기능을 제공한다.
    - 이 공정성은 오래 기다린 스레드에 락을 줄 수 있는 설정이다.

<br/>

<br/>

- 코드
    - 락을 사용할 수 있거나 `lock()` 을 호출한 스레드가 이미 락을 소유한 경우 → **재진입**
    
    ```java
    Lock key = new ReentrantLock();
    
    key.lock(); 
    try {
    		/* critical section */
    		//예외가 발생하면 락이 해제되는 것을 보장
    }
    finally {
    		key.unlock(); 
    }
    ```
    

<br/>

<br/>

> `ReentrantLock` 은 상호 배제를 제공하지만 여러 스레드가 공유 데이터를 읽기만 할 때에는 너무 보수적인 전략일 수 있다.
이를 위해 Java API는 `ReentrantReadWriteLock` 을 제공한다.
> 

<br/>

<br/>

### 세마포

- 상호 배제를 위해 세마포를 사용하는 방법
    
    ```java
    Semaphore sem = new Semaphore(1);
    
    try {
    		sem.acquire();
    		/* critical section */
    }
    catch (InterruptedException ie) { } 
    finally {
    		sem.release(); //세마포가 반드시 해제되도록 finally 절 안에 작성
    }
    ```
    

<br/>

<br/>

### 조건 변수

> 조건 변수는 wait() 및 notify() 메소드와 유사한 기능을 제공한다.
따라서 상호 배제를 제공하려면 조건 변수를 재진입 락과 연관시켜야 한다.
> 

<br/>

<br/>

- 초기 과정
    - 먼저 ReentrantLock을 생성 후 조건 변수를 생성
    
    ```java
    Lock key = new ReentrantLock();
    Condition condVar = key.newCondition();
    ```
    

<br/>

<br/>

- Java 조건 변수를 이용한 예제
    
    ```java
    /* threadNumber is the thread that wishes to do some work */ 
    public void doWork(int threadNumber) {
    		lock.lock();
    
    		try {
    		/**
    	      * If it’s not my turn, then wait
    	      * until I’m signaled.
    	      */
    				if (threadNumber != turn) 
    						condVars[threadNumber].await();
    
    		/**
    	      * Do some work for awhile ...
    	      */

    	     /**    
    	      * Now signal to the next thread.
    	      */
    		    turn = (turn + 1) % 5;
    				condVars[turn].signal();
    		}
    		catch (InterruptedException ie) { } 
    		finally {
    	     lock.unlock();
    		}
    }
    ```


<br/>

<br/>

## 💡 대체 방안들 

### 트랜잭션 메모리

> 컴퓨터 과학 분양에서는 매우 자주 한 연구 분야의 아이디어가 다른 분야의 문제를 해결하곤 한다.
> 
> - **트랜잭션 메모리**의 개념은 데이터베이스 이론 분야에서 출발한 아이디어지만, 프로세스 동기화 전략을 제공한다.

<br/>

<br/>

> **메모리 트랜잭션**은 메모리 읽기와 쓰기 연산의 원자적인 연속적 순서이다.
> 
> - 한 트랜잭션의 모든 연산이 완수되면 확정(`commit`)된다.
> - 그렇지 않다면? → 이전의 상태로 되돌려야(`roll-back`) 한다.

<br/>

<br/>

- 예시 코드 1
    - 아래 코드의 경우, 교착 상태와 같은 많은 잠재적인 문제를 야기할 수 있다.
    - 또한 스레드의 개수가 증가할수록 락을 소유하기 위한 스레드의 경쟁 수준이 매우 높아진다.
        
        ```java
        void update() {
        		acquire();
        
        		//공유 데이터 변경
        
        		release();
        }
        ```
        

<br/>

<br/>

- 예시 코드 2
    - *이러한 기법은 개발자가 아니라 트랜잭션 메모리 시스템이 원자성을 보장할 책임을 진다는 것이다!*
    - 또한 락이 전혀 사용되지 않고 있어, 교착 상태가 발생하는 것이 불가능하다.
        
        ```java
        void update() {
        		atomic {
        				//공유 데이터 변경
        		}
        }
        ```
        

<br/>

<br/>

> 트랜잭션 메모리는 소프트웨어 또는 하드웨어로 구현될 수 있다.
> 
> - 이름이 나타내는 것처럼 **소프트웨어 트랜잭션 메모리(STM)**는 소프트웨어만으로 구현된다.
>     - 이는 트랜잭션 블록 안에 검사 코드를 삽입함으로써 동작한다.
> 
> - **하드웨어 트랜잭션 메모리(HTM)**는 하드웨어만으로 구현된다.
>     - 개별 처리기 캐시에 존재하는 공유 데이터의 충돌을 해결하고 관리하기 위해 하드웨어 캐시 계층 구조와 캐시 일관성 프로토콜을 사용한다.
>     - 코드 계측이 필요 없고, 따라서 STM보다 적은 오버헤드를 가진다.

<br/>

<br/>

### OpenMP

> OpenMP는 컴파일러 디렉티브와 API로 구성된다는 것을 기억하기를 바란다.
> 
> - OpenMP가 가지는 장점
>     - 스레드의 생성과 관리가 라이브러리에 의해 처리되어 응용 개발자들이 신경쓰지 않아도 된다!

<br/>

<br/>

> OpenMP는 `#pragma omp critical` 이라는 디렉티브를 제공한다.
> 
> - 이 디렉티브는 디렉티브 이후에 나오는 코드구역을 임계구역으로 지정한다.
> - 이런 식으로 스레드가 경쟁 조건을 발생시키지 않는다는 것을 보장한다.

<br/>

<br/>

- 사용법
    
    ```java
    void update(int value) {
    		#pragma omp critical
    		{
    				counter += value;
    		}
    }
    ```
    

<br/>

<br/>

### 함수형 프로그래밍 언어

> 사람들이 많이 아는 C, C++, Java는 **명령형** 또는 **절차형** 언어라고 한다.
> 
> - 명령형 언어 → 상태를 기반으로 둔 알고리즘을 구현하는 데 사용

<br/>

<br/>

> 함수형 프로그래밍 언어란?
> 
> - 명령형 언어가 제공하는 패러다임과 많이 다르다.
>     - 근본적인 차이 → 함수형 언어는 상태를 유지하지 않는다.
>     - 즉, 변수가 정의되어 값을 배정받으면 그 값은 변경될 수 없기 때문에 변하지 않는다.

<br/>

<br/>

- 함수형 언어의 예시로는 Erlang과 Scala가 있다.
    - Erlang
        - 병행성과 병렬 시스템에서 실행되는 응용을 개발하기 쉬운 언어로 주목받음.
    - Scala
        - 함수형 언어이면서 객체지향이기도 하다.
        - 많은 분법이 Java와 C#과 유사하다.