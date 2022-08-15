
> 따로 정리한 PPT 자료는 [링크](https://github.com/2dongyeop/TIL/blob/main/Java/JVM-and-GC.pdf)를 통해 확인하실 수 있습니다.

<br/>

## JVM이란?
- Java Virtual Machine의 줄임말로, 직역하면 '자바를 실행하기 위한 가상 기계'라고 할 수 있다.

<br/>

## Java 프로그램의 구동 과정을 통해 알아보는 JVM의 역할
<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/java-run.png" width = 500/>

1. 사용자가 확장자가 `.java` 인 소스 파일을 작성한다.
2. 자바 컴파일러를 통해 확장자가 `.class` 파일인 바이트 코드 파일을 생성한다.
    - 이때 완성된 파일은 완전한 기계어가 아니기 때문에, 운영체제가 바로 실행할 수 없어 이를 위한 가상의 운영체제가 필요하다.
3. 이후 바이트 코드 파일은 JVM 구동 명령어에 의해 해석되고 해당 운영체제에 맞게 기계어로 번역된다.

<br/>

## JVM 구성 요소
<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/JVM.png" width = 500/>

### 1. 클래스 로더
- JVM 내로 클래스 파일(`.class`)을 로드하고, 링크를 통해 배치하는 작업을 수행한다.
    - 즉, 클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크하는 역할을 한다.

<br/>

### 2. 실행 엔진
- 클래스 로더가 JVM 내의 런타임 데이터 영역에 바이트 코드를 배치시키면, 실행 엔진에 의해 실행된다.
    - 간단히 말해, 클래스를 실행시키는 역할로, 3가지 요소로 구성되어 있다.

<br/>

#### 인터프리터

- 코드를 한 줄씩 실행하는 방식으로 동작하며, 한 번에 실행하는 컴파일러에 비해 속도가 느리다.

<br>

#### JIT(Just-In-Time) 컴파일러

- 인터프리터 방식으로 실행하다가 적절한 시점에 바이트 코드 전체를 컴파일하여 기계어로 변경
- 이후에는 인터프리팅 하지 않고 기계어로 직접 실행하는 방식이다.

<br>

#### 가비지 콜렉터

- 더이상 사용되지 않는 인스턴스를 찾아 메모리에서 삭제한다.

<br/>

### 3. 런타임 데이터 영역
- 프로그램을 수행하기 위해 OS로부터 할당받은 메모리 공간을 말한다.

<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/Runtime-Data-Area.png" width = 500/>

<br/>

#### PC Register

- Thread가 어떤 명령을 실행할 지에 대한 내용을 기록하는 부분이다.

<br>

#### JVM Stack 영역

- 프로그램 실행 과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 데이터를 저장한다.
    - 매개변수, 지역변수 등이 해당된다.

<br>

#### Native Method Stack

- 자바 프로그램이 컴파일되어 생성되는 바이트 코드가 아닌, 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역이다.
    - 즉, Java가 아닌 다른 언어로 작성된 코드를 위한 공간이다.

<br>

#### Method Area

- 클래스 정보를 처음 메모리 공간에 올릴 때 **초기화되는 대상을 저장하기 위한 메모리 공간**

<br/>

#### Heap Area

<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/Heap-Area.png" width = 500/>

- 객체나 배열을 저장하는 가상메모리 공간이다.
- 참조되지 않는 객체가 있으면 Garbage Collection이 일어난다.

<br/>

---

## GC : Garbage Collection
- Java에서는 Garbage Collector가 필요없는 객체를 찾아 지우는 작업을 수행하고,
- 이 작업을 Garbage Collection이라고 한다.

<br/>

### GC의 주요 동작 2가지
1. 힙(heap)영역 내에 가비지(garbage)를 찾아낸다.
2. 찾아낸 가비지를 회수하여 힙의 메모리를 회수한다.

<br/>

### 알아야 할 2가지 용어
💡 stop-the-world
- GC를 실행시키기 위해 JVM이 애플리케이션 실행을 멈추는 것
- stop-the-world가 실행되면 GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다. 
- GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다. 

<br/>

💡 Mark and Sweep
- GC가 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각 어떤 객체를 참조하는지 찾는 과정이 `Mark`이고, 이 과정에서 `stop-the-world`가 발생한다. 
- 이후 Mark 되어있지 않는 객체들을 힙에서 제거하는 과정이 `Sweep`이다.

<br/>

### Heap 영역내에서 발생하는 GC
<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/Heap-Area.png" width = 500/>

<br/>

### Young 영역
- Eden 영역과 Survivor1, Survivor2 영역으로 구성된다.
    - Eden 영역에는 주로 새롭게 생성한 객체의 대부분이 위치한다.
    - Survivor0 영역에는 Eden 영역에서 GC 발생 후, 살아남은 객체가 이동한다.
    - 수차례에 걸쳐 Survivor0에 객체가 쌓여 가득 찰 경우, Survivor1 영역으로 이동한다.
- 이 영역에서 발생하는 GC를 Minor GC라고 칭한다.

<br/>

### Old 영역
- Old 영역은 기본적으로 가득 찰 경우에 GC를 실행한다.
    - 이 안에는 Young 영역에서 살아남은 객체들이 존재한다.
    - GC 방식에는 7가지가 있으며, 각 방식별로 처리 절차가 다르다.

<br/>

### Permanent 영역
- 클래스와 메소드의 메타데이터를 저장하는 영역이다.
- JDK 8부터 Heap 영역은 삭제되었다.