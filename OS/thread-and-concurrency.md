# 스레드와 병행성

>💡 ***이 장의 목표***
>
>- *스레드이 기본 구성요소를 식별하고 스레드와 프로세스를 대조한다.*
>- *다중 스레드 프로세스를 설계할 때의 주요 이점과 중대한 과제를 설명한다.*
>- *스레드 풀, 포크 조인 및 그랜드 센트럴 디스패치를 포함하여 암시적 스레딩에 대한 다양한 접근 방식을 설명한다.*

<br/>

<br/>

## 💡 개요


>💡 스레드는 CPU의 이용 기본 단위로, 스레드 ID, 프로그램 카운터, 레지스터 집합, 스택으로 구성된다.
>
>스레드는 같은 프로세스에 속한 다른 스레드와 운영체제 자원들을 공유한다.
>
>단일 스레드 프로세스와 다중 스레드 프로세스의 차이점을 알아보자!

<br/>

<br/>

### 동기

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/single-multi-thread.png" width = 600/>

- 현대의 컴퓨터와 모바일 기기에서 작동하는 거의 모든 소프트웨어 응용들은 다중 스레드를 이용한다.
    - 하나의 응용은 몇 개의 실행 흐름을 가진 독립적인 프로세스로 구현된다.
    - ex) 웹 브라우저 : 이미지를 띄우고, 텍스트를 표시하고, 데이터 검색을 동시에..

<br/>

<br/>

- 응용은 다중 코어 시스템에서 처리 능력을 향상시키도록 설계될 수 있다.
    - 이러한 응용은 다중 계산 코어를 사용하여 다수의 CPU-집중 작업을 병렬로 처리할 수 있다.

<br/>

<br/>

- 하나의 응용 프로그램이 여러 개의 비슷한 작업을 수행할 필요가 있는 상황들이 있을 수 있다!
    - ex) 수천개의 클라이언트가 하나의 웹서버에 병행 접근하는 상황
    - 이때, 웹 서버가 전통적인 단일 스레드 프로세스라면? → 매우 긴 대기 시간이 발생

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/multithread-server.png" width = 600/>

<br/>

<br/>

- 해결법
    - ***서버가 요청을 받아들이는 하나의 프로세스로 동작하게 하자!***
    - ⇒ 서버에게 서비스 요청이 들어오면, 프로세스는 그 요청을 수행할 별도 프로세스로 생성!
    - ***단, 이 방식은 기존 프로세스와 하는 일이 동일하다면? → 여러 스레드를 만들어나가자.***

<br/>

<br/>

- 많은 응용 프로그램도 기본 정렬, 트리 및 그래프 알고리즘을 포함하여 다중 스레드를 활용할 수 있다.

<br/>

<br/>

### 장점

> 다중 스레드 프로그래밍의 이점은 다음의 4가지 큰 부류로 나눌 수 있다.
 

1. ***응답성(responsiveness)***
    1. 대화형 응용을 다중 스레드화 → 응용의 일부가 봉쇄되거나, 긴 작업을 수행하도록 허용 → 응답성 증가
    2. 이 특징은 사용자 인터페이스를 설계하는 데 있어 특히 유용하다.
2. ***자원 공유(resource sharing)***
    1. 스레드는 자동으로 그들이 속한 프로세스의 자원들과 메모리를 공유한다.
3. ***경제성(economy)***
    1. 프로세스 생성에 비해 스레드를 생성하고 문맥 교환하는 것은 훨씬 경제적이다.
4. ***규모 적응성(scalability)***
    1. 다중 처리기 구조에서는 각각의 스레드가 다른 처리기에서 병렬로 수행할 수 있어 이점이 더 크다.


<br/>

<br/>

## 💡 다중 코어 프로그래밍

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/single-core.png" width = 600/>

- 컴퓨터는 점차 발전하여 단일 컴퓨팅 칩에 여러 컴퓨팅 코어를 배치하기 시작했다.
    - 각 코어는 운영체제에 별도로 CPU로 보이는데, 이를 **다중 코어** 시스템이라고 한다.
    - ***다중 스레드 프로그래밍은 이런 여러 컴퓨팅 코어를 보다 효율적으로 사용하고 병행성을 향상시킨다!***

<br/>

<br/>

- 현재 논의에서 ***병행성***과 ***병렬성***의 차이점에 주목하자.
    - **병행** : 모든 작업이 진행되게 하여 둘 이상의 작업을 지원한다.
    - **병렬** : 둘 이상의 작업을 동시에 수행할 수 있다.

<br/>

<br/>

### 프로그래밍 도전과제

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/multi-core.png" width = 600/>


- 프로그래밍 도전 과제
    - 운영체제 설계자 : 병렬 수행이 될도록 여러 코어를 활용하는 스케줄링 알고리즘을 개발해야 한다.
    - 응용 프로그래머 : 기존 프로그램을 다중 스레드를 사용할 수 있도록 수정해야 한다.

<br/>

<br/>

- 다중 코어 시스템을 위해 프로그래밍하기 위한 5개의 ***극복해야 할 도전 과제!***
    1. **태스크 인식(identifying tasks)**
        1. 응용을 분석하여 독립된 병행 가능 태스크로 나눌 수 있는 영역을 찾을 수 있어야 한다.
    2. **균형(balance)**
        1. 찾아진 부분들이 전체 작업에 균등한 기여도를 가지도록 태스크로 나누는 것도 매우 중요하다.
    3. **데이터 분리(data spliting)**
        1. 응용이 독립된 태스크로 나누어지듯, 태스크가 접근하고 조작하는 데이터도 나누어져야 한다.
    4. **데이터 종속성(data dependency)**
        1. 태스크가 접근하는 데이터는 둘 이상의 태스크 사이에 종속성이 없는지 검토되어야 한다.
    5. **시험 및 디버깅(testing and debugging)**
        1. 프로그램이 다중 코어에서 병렬로 실행될 때, 다양한 실행 경로가 존재할 수 있다.
        2. 단일 스레드 응용의 시험 및 디버깅보다 훨씬 어렵지만, 필수적이다.

<br/>

<br/>

### 병렬 실행의 유형

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/data-and-task-parallelism.png" width = 600/>


<br/>

<br/>

- ***데이터 병렬 실행***
    - 동일한 데이터를 다수의 계산 코어에 분배한 뒤 각 코어에서 동일한 연산을 실행한다.

<br/>

<br/>

- ***태스크 병렬 실행***
    - 데이터가 아니라 태스크(스레드)를 다수의 코어에 분배한다.
    - 각 스레드는 고유의 연산을 실행한다.

<br/>

<br/>

- *데이터와 태스크 병렬 처리는 상호 배타적이지 않고, 실제론 두 가지 전략을 혼합하여 사용할 수도 있다!*

<br/>

<br/>

## 💡 다중 스레드 모델

- 스레드를 위한 지원
    - **사용자 스레드** : 커널 위에서 지원되며 커널의 지원 없이 관리된다.
    - **커널 스레드** : 운영체제에 의해 직접 지원되고 관리된다.

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/user-kernel-thread.png" width = 600/>

- 사용자 스레드와 커널 스레드는 어떤 연관 관계가 존재해야 한다.
    - ***이 연관 관계를 확립하는 세 가지 일반적인 방법을 살펴보자!***

<br/>

<br/>

### 다대일 모델

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/many-to-one-model.png" width = 600/>

- 다대일 모델 : 많은 사용자 수준 스레드를 하나의 커널 스레드로 사상한다.
    - 스레드 관리는 사용자 공간의 스레드 라이브러리에 의해 행해지므로 효율적이다!
    - **다만, 한 스레드가 봉쇄형 시스템을 콜 할 경우 → 전체 프로세스가 봉쇄**
    - 또한, 한 번에 하나의 스레드만이 커널에 접근 가능 → 다중 스레드가 다중 코어에서 병렬로 실행 불가
    - ex) 그린 스레드(green thread)

<br/>

<br/>

### 일대일 모델

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/one-to-one-model.png" width = 600/>

- 일대일 모델 : 각 사용자 스레드를 각각 하나의 커널 스레드로 사상한다.
    - 하나의 스레드가 봉쇄적 시스템 콜을 호출해도 다른 스레드가 영향을 받지 않음 → 병렬성 제공!
    - 다중 처리기에서 다중 스레드가 병렬로 수행되도록 허용!
    - **단, 사용자 스레드를 만들 때마다 커러 스레드를 만듦 → 시스템 성능에 부담을 줌**

<br/>

<br/>

### 다대다 모델

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/many-to-many-model.png" width = 600/>

- 다대다 모델 : 여러 개의 사용자 수준 스레드를 같거나 작은 수의 커널 스레드로 멀티플렉스한다.
    - 다대다 모델은 앞에 두 모델의 단점들을 어느정도 해결한다.
    - 개발자는 필요한 수의 사용자 스레드를 만들 수 있고, 커널 스레드가 병렬로 수행될 수도 있다.
    - 또한 봉쇄형 시스템콜이 일어나도 다른 스레드는 영향을 받지 않는다.
    - 이 변형을 때로 **두 수준 모델(two-level model)**이라고도 부른다.

<br/>

<br/>

## 💡 스레드 라이브러리

- **스레드 라이브러리(threads library)**
    - 프로그래머에게 스레드를 생성 및 관리하기 위한 API를 제공한다.

<br/>

<br/>

- 스레드 라이브러리를 구현하는 두 가지 방법
    1. 커널의 지원 없이 완전히 사용자 공간에서 라이브러리 제공하기
        1. ⇒ 라이브러리의 모든 코드와 자료구조를 사용자 공간에 존재하게 한다.
        2. ***라이브러리 함수 호출 ≠ 시스템 콜을 의미.***
    2. 운영체제에 의해 지원되는 커널 수준 라이브러리를 구현하기
        1. ⇒ 라이브러리를 위한 코드와 자료구조는 커널 공간에 존재한다.
        2. ***라이브러리 함수 호출 == 시스템 콜을 의미.***

<br/>

<br/>

- 현재 자주 사용되는 세 종류의 라이브러리
    - POSIX Pthreads
    - Windows
    - Java

<br/>

<br/>

- *다수의 스레드를 생성하는 **비동기 스레딩**과 **동기 스레딩**의 전략을 알아보자!*
    - **비동기 스레딩** : 부모가 자식을 생성 후 부모와 자식이 서로 독립적으로 병행 실행
        - 독립적이기 때문에 데이터 공유는 거의 없다.
    - **동기 스레딩** : 부모가 자식을 생성 후 자식들이 종료할 때까지 기다린다.
        - 통상 동기 스레딩은 많은 양의 데이터 공유를 수반한다.

<br/>

<br/>

### PThreads

- Pthreads란?
    - POSIX가 스레드 생성과 동기화를 위해 제정한 표준 API
    - ***스레드의 동작에 관한 명세일 뿐이지 그것 자체를 구현한 것은 아니다!***

<br/>

<br/>

- PThread 규칙
    - 별도의 스레드는 지정된 함수(`runner()`)에서 실행을 시작한다.
    - 모든 PThreads 프로그램은 `pthread.h` 헤더 파일을 포함해야 한다.

<br/>

<br/>

### Windows 스레드

- 이 라이브러리를 이용하여 스레드를 생성하는 기술은 Pthreads와 매우 유사하다!

<br/>

<br/>

- 동작 방식
    - `CreateThread()` 함수에 의해 스레드를 생성하고, 이 함수에 스레드를 위한 속성들이 전달된다.
    - Windows API에서는 `WaitForSingleObject()` 를 이용해 부모 스레드가 합산 스레드를 기다린다.
        - 이때, 아래와 같이 4개의 매개변수를 전달받는다.
            - 기다려야 하는 객체의 개수
            - 객체 배열을 가리키는 포인터
            - 모든 객체가 신호를 보내왔는지를 나타내는 플래그
            - 대기해야 하는 타임아웃 시간

<br/>

<br/>

### Java 스레드

> 스레드는 Java 프로그램 실행의 근본적인 모델이다.
>
> Java 언어와 API는 스레드의 생성과 관리를 지원하는 풍부한 특성을 제공한다.


<br/>

<br/>

- Java 프로그램에서 스레드를 명시적으로 생성하는 두 가지 기법
    1. Thread 클래스에서 파생된 새 클래스를 만들고, `run()` 메소드를 재정의
    2. Runnable 인터페이스를 구현하는 클래스를 제정의하기

<br/>

<br/>

- 스레드 만들기 예제 코드
    - Thread 객체를 생성할 때, Runnable을 구현하는 클래스의 인스턴스를 전달하기
    
    ```java
    Thread worker = new Thread(new Tash());
    worker.start();
    ```
    
<br/>

<br/>


- 새 스레드 객체에서 `start()` 메소드를 호출하는 두 가지 작업이 수행된다.
    1. 메모리가 할당되고, JVM 내에 새로운 스레드가 초기화된다.
    2. `run()` 메소드가 호출하면 스레드가 JVM에 의해 수행될 자격을 갖게 된다.

<br/>

<br/>

> **Executor 프레임워크**
> 
- Java는 스레드 생성 및 통신에 대한 제어 기능을 크게 향상시키는 병행 처리 기능을 도입했다.
    - Thread 객체를 명시적으로 생성하는 대신 `Executor` 인터페이스를 중심으로 스레드를 생성한다.
        
        ```java
        public interface Executor {
        		void execute(Runnable command);
        }
        ```
        
<br/>

<br/>


- 스레드 만들기 예제 코드
    - `execute()` 메소드를 호출할 때, Runnable 객체가 인자로 전달해야 한다.
    - 별도의 Thread 객체를 만들고 `start()` 메소드를 호출하는 대신 Executor를 사용하는 것을 의미.
    
    ```java
    Executore service = new Executor();
    service.execute(new Task());
    ```
    
<br/>

<br/>


- Executor 프레임워크는 ***생성자-소비자 모델을 기반***으로 한다.
    - Runnable 인터페이스를 구현하는 작업이 생성되고, 이러한 작업을 실행하는 스레드가 이를 소비한다.
    - 이로써 ***스레드 생성을 실행에서 분리***할 뿐만 아니라 ***병행하게 실행되는 작업의 통신 기법도 제공***한다!


<br/>

<br/>

## 💡 암묵적 스레딩

> 💡 다중 코어 처리의 지속적 성장에 따라 수백 또는 수천 개의 스레드를 가진 응용이 등장했다.
>
> ***이를 설계하는 것에 대한 어려움을 극복하고, 병행 및 병렬 설계를 도와주는 방식에는 암묵적 스레딩이 있다.***

<br/>

<br/>

- **암묵적 스레딩**이란?
    - ***스레딩의 생성과 관리 책임을 응용 개발자로부터 컴파일러와 실행시간 라이브러리에게 넘겨주는 것!***

<br/>

<br/>

### 스레드 풀

- **다중 스레드 서버는 아직도 여러 문제를 가지고 있다!**
    1. 서비스할 때마다 스레드를 생성하는 데 소요하는 시간
    2. 모든 요청마다 새 스레드를 만들어서 서비스를 한다? 
        
        → 시스템에서 동시에 실행할 수 있는 최대 스레드 수가 몇 개 까지 가능할 수 있는 설정해야 함
        
    - 따라서 이 모든 문제를 해결하는 방법 중 하나가 ***스레드 풀!***

<br/>

<br/>

- 스레드 풀의 기본 아이디어
    - 프로세스를 시작할 때 ***일정 수의 스레드를 미리 풀로 만들어두자!***
    - 사용 가능한 스레드가 있을 때 → 요청을 받으면 즉시 서비스가 진행된다.
    - 사용 가능한 스레드가 없을 때 → 사용 가능 스레드가 생길 때까지 기다린다.

<br/>

<br/>

- 스레드 풀의 장점
    1. 기존 스레드로 서비스를 해주면 종종 빠르다.
    2. 스레드 풀은 존재할 스레드 개수에 제한을 두어, 시스템에 도움이 된다.
    3. 태스크를 생성하는 방법을 태스크로부터 분리하여 태스크 실행을 다르게 할 수 있다.

<br/>

<br/>

### Fork Join

- ***fork-join*** 모델의 동작 방식
    - 메인 부모 스레드가 자식을 생성(`fork`)한 뒤, 자식의 종료가 끝나면 `join` 후에 동작할 수 있다.
    - 이 동기식 모델은 종종 명시적 스레드 생성이라고 특징지어지지만, 암시적 스레딩에도 사용된다.

<br/>

<br/>

- Java 에서의 Fork Join
    - 자바는 재귀-정복 알고리즘과 함께 사용되도록 설계된 1.7v API에 fork-join 라이브러리를 도입했다!
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/Fork-join.png" width = 600/>  

<br/>

<br/>

### OpenMP

- OpenMP란?
    - C, C++ 등으로 작성된 API와 컴파일러 디렉티브의 집합으로,
    - 공유 메모리 환경에서 병렬 프로그래밍을 할 수 있도록 도움을 준다.

<br/>

<br/>

- 동작 방식
    - OpenMP는 병렬로 실행될 수 있는 블록을 찾아 ***병렬 영역(parallel regions)***이라고 부른다.
    - 응용 개발자는 자신들의 코드 중 병렬 영역에 컴파일러 디렉티브를 삽입한다.

<br/>

<br/>

- 코드 설명
    
    ```java
    #pragma omp parallel for
    ```
    
    - 병렬화를 위한 디렉티브를 제공하며, 추가로 OpenMP는 ***개발자가 병렬화 수준을 선택할 수 있게*** 한다.

<br/>

<br/>

### Grand Central Dispatch

- `GCD(Grand Central Dispatch)`란?
    - macOS 및 iOS를 위해 Apple에서 개발한 기술이다.
    - 개발자가 병렬로 실행될 코드 섹션(***태스크***)을 식별할 수 있도록 런타임 라이브러리, API 및 언어 확장의 조합이다.

<br/>

<br/>

- 내부 구성
    - 런타임 수행을 위해 태스크를 디스패치 큐에 넣어서 스케줄링 한다.
    - 큐에서 태스크를 제거할 때 관리하는 스레드풀에서 가용 스레드를 선택하여 태스크를 할당한다.
    - *직렬(serial)*과 *병렬(concurrent) 디스패치 큐***를 유지한다.

<br/>

<br/>

- 직렬 큐의 동작 방식
    - ***FIFO*** 순으로 제거된다.
    - 태스크는 큐에서 제거되면 다른 태스크가 제거되기 전에 반드시 실행을 완료해야 한다!
    - 각 프로세스는 고유한 직렬 큐(**메인 큐, 개인 디스패치 큐**)가 있다.
    - 여러 작업을 순차적으로 실행하는 데 유용!

<br/>

<br/>

- 병렬 큐의 동작 방식
    - ***FIFO*** 순으로 제거되지만, ***한 번에 여러 태스크가 제거되어 병렬로 실행***한다.
    - 다수의 시스템 전체의 병행 큐(전역 디스패치 큐)가 존재하며, 4가지 서비스 품질 클래스로 나뉜다.
        - `QOS_CLASS_USER_INTEFACE` : 사용자 대화형 클래스
        - `QOS_CLASS_USER_INITIATED` : 사용자 시작 클래스
        - `QOS_CLASS_UTILITY` : 유틸리티 클래스
        - `QOS_CLASS_BACKGROUND` : 백그라운드 클래스

<br/>

<br/>

### Intel 스레드 빌딩 블록

- `Intel TBB(threading building block)`이란?
    - C++에서 병렬 응용 프로그램 설계를 지원하는 템블릿 라이브러리.

<br/>

<br/>

- 기능
    - 개발자가 병렬로 실행할 수 있는 태스크를 지정하게 하고, 
    TBB 태스크 스케줄러는 이러한 태스크를 하부 스레드에 매핑한다.
    - 태스크 스케줄러는 부하 균형 기능을 제공하고 캐시를 인지한다!
        
        → 이는 캐시 메모리에 데이터가 저장되어 보다 빠르게 실행되는 태스크에 우선순위를 부여함을 의미
        
    - 이외엔 병렬 루프 구조, 상호 배제 잠금 등등을 위한 템플릿을 포함하여 다양한 기능을 제공한다.


<br/>

<br/>

## 💡 스레드와 관련된 문제들 

### `Fork()` 및 `Exec()` 시스템 콜

- 다중 스레드 프로그램에서는 `fork()`와 `exec()`의 의미가 달라질 수 있다!
    - `fork()` : 모든 스레드를 복제해야 하는가? or 한 스레드만 복제해야 하는가?
    - `exec()` : 모든 스레드를 다 실행해야 하는가? 아니면 한 스레드만 실행해야 하는가?

<br/>

<br/>

### 신호 처리

- **신호**는 UNIX에서 프로세스에 어떤 이벤트가 일어났음을 알려주기 위해 사용된다.
    - 신호는 알려줄 이벤트의 근원지나 이유에 따라 ***동기식 또는 비동기식***으로 전달될 수 있다.

<br/>

<br/>

- 동기식이건 비동기식이건 모든 신호는 아래와 같은 형태로 전달된다.
    1. 신호는 특정 이벤트가 일어나야 생성된다.
    2. 생성된 신호가 프로세스에 전달된다.
    3. 신호가 전달되면 반드시 처리되어야 한다.

<br/>

<br/>

- 신호의 전달
    - 동기 신호 : 신호를 방생시킨 연산을 수행한 동일 프로세스에 전달된다.
    - 비동기 신호 : 통상 다른 프로세스에 전달된다.

<br/>

<br/>

- 모든 신호는 둘 중 하나의 처리기에 의해 처리된다.
    1. 디폴트 신호 처리기
    2. 사용자 정의 신호 처리기

<br/>

<br/>

- 모든 신호마다 커널이 실행시키는 **디폴트 처리기**가 있다!
    - 이는 신호를 처리하기 위하여 호출되는 **사용자 정의 처리기**에 의해 대체될 수 있다.

<br/>

<br/>

- 만약, 단일 스레드가 아닌 다중 스레드 프로그램에 신호가 전달되면 어디에 전달해야 하는가?
    - 이 고민에는 아래와 같은 선택이 존재한다.
        1. 신호가 적용될 스레드에게 전달한다.
        2. 모든 스레드에 전달한다.
        3. 몇몇 스레드들에만 선택적으로 전달한다.
        4. 특정 스레드가 모든 신호를 전달받도록 지정한다.

<br/>

<br/>

### 스레드 취소

- 스레드 취소(***thread cancellation***)란?
    - 스레드가 끝나기 전에 강제 종료시키는 작업이다.
    - ex) 병렬로 데이터베이스를 검색하다가 한 스레드가 결과를 찾으면 나머지 스레드를 취소.

<br/>

<br/>

- **목적 스레드**(`target thread`)란?
    - 취소되어야 할 스레드를 일컫는 말로, 목적 스레드의 취소는 아래 두 가지 방식으로 발생한다.
    - *비동기식 취소(asynchronous cancellation)*
        - 한 스레드가 즉시 목적 스레드를 강제 종료시킨다.
    - *지연 취소(deferred cancellation)*
        - 목적 스레드가 주기적으로 자신이 강제 종료되어야 할 지를 점검한다.

<br/>

<br/>

- 비동기식 취소
    - 스레드가 다른 스레드와 공유하는 자료구조를 갱신하는 도중에 취소 요청이 올 경우 문제가 된다.
    - 이 경우, 필요한 시스템 자원을 다 회수하지 못할 뿐더러 자원을 다 사용 가능한 상태로 못 만들수도 있음!

<br/>

<br/>

- 지연 취소
    - 실제 취소는 목적 스레드가 취소 여부를 결정하기 위한 플래그를 검사한 이후에야 일어난다.
    - 따라서 스레드가 자신이 취소되어도 안전하다고 판단되는 시점에서 취소 여부를 검사한다.

<br/>

<br/>

### 스레드-로컬 저장장치

- 한 프로세스에 속한 스레드들은 그 프로세스의 데이터를 모두 공유한다.
    - 이는 다중 스레드 프로그래밍의 큰 장점 중 하나지만, ***상황에 따라서는 자신만 액세스할 수 있는 데이터가 필요***하다.
    - 이러한 데이터를 **스레드-로컬 저장장치(`TLS`)**라고 부른다.

<br/>

<br/>

- 어떤 면에서 TLS는 정적 데이터와 유사하다.
    - 차이점은 TLS 데이터는 스레드마다 고유하다는 것이다!

<br/>

<br/>

### 스케줄러 액티베이션

- 다중 스레드 프로그램의 마지막 문제는 ***스레드 라이브러리와 커널의 통신 문제***이다.
    - 이 통신의 조정은 응용 프로그램이 최고의 성능을 보이도록 보장하기 위해 커널 스레드의 수를 동적으로 저잘하는 것을 가능하게 한다.

<br/>

<br/>

- 다대다 or 두 수준 모델을 구현하는 많은 시스템은 사용자와 커널 스레드 사이에 중간 자료구조를 둔다.
    - 이 자료구조는 통산 **경량 프로세스** 또는 **LWP**라고 불린다.
    - LWP는 하나의 커널 스레드에 부속되어 있다.
        - 물리 처리기에서 스케줄하는 대상은 바로 이 커널 스레드이다.
        
<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/LWP.png" width = 600/>
        

<br/>

<br/>

- ***스케줄러 액티베이션***
    - 사용자 스레드 라이브러이와 커널 스레드 간의 통신 방법 중 하나이다.
    - 커널은 응용에 가상 처리기의 집합을 제공하고 응용은 사용자 스레드를 가용한 가상 처리기로 스케줄한다.

<br/>

<br/>

- 게다가 커널은 응용에게 특정 이벤트에 대해 알려줘야 한다!
    - 이 프로시저를 **upcall**이라고 부른다.
    - Upcall은 upcall 처리기에 의해 처리되고, **upcall 처리기**는 가상 처리기상에서 실행된다.

<br/>

<br/>

## 💡 운영체제 사례

### Windows 스레드

- Windows 응용들은 프로세스 형태로 실행되며, 이들 각 프로세스는 한 개 또는 그 이상의 스레드를 가진다.
    - Windows 스레드의 일반적인 구성 요소는 아래와 같다.
        - 각 스레드를 유일하게 지목하는 스레드 ID
        - 처리기의 상태를 나타내는 레지스터 집합
        - 프로그램 카운터
        - 사용자 모드에서 실행될 때 필요한 사용자 스택, 커널 모드에서 실행될 때 필요한 커널 스택
        - 실행 시간 라이브러리와 동적 링크 라이브러리(DDL) 등이 사용하는 개별 데이터 저장 영역

<br/>

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/Windows-thread.png" width = 600/>

- 레지스터 집합, 스택, 개별 데이터 저장 영역들은 그 스레드의 문맥으로 불린다.
    - 스레드의 주요 자료구조는 다음과 같다.
        - **ETHREAD** - 실행 스레드 블록(`executive thread block`)
        - **KTHREAD** - 커널 스레드 블록(`kernel thread block`)
        - **TEB** - 스레드 환경 블록(`thread environment block`)

<br/>

<br/>

### Linux 스레드

- 리눅스는 아래 두 가지 기능을 지원한다.
    - `fork()` : 프로세스를 복제하는 기능
    - `clone()` : 스레드를 생성할 수 있는 기능
    
    ***→ 그러나, Linux는 프로세스와 스레드를 구별하지 않는다!***
    
    → 프로그램 내의 제어 흐름을 나타내기 위해 ***태스크***라는 용어를 사용한다!
    

<br/>

<br/>

- `clone()` 이 호출될 때 부모와 자식 태스크가 자료구조를 얼마나 공유할 지 결정하는 플래그는 아래와 같다.

    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/Linux-flag.png" width = 600/>    