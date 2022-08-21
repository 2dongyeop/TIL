## PCB(Process Control Block)이란?
- 프로세스 메타데이터들을 저장해 놓는 곳으로, 한 PCB 안에는 한 프로세스의 정보가 담긴다.

<br/>

### PCB의 용도
- CPU에서는 프로세스의 상태에 따라 교체작업이 이루어진다.
  - 이때, 앞으로 다시 수행할 대기 중인 프로세스에 관한 저장 값을 PCB에 저장해두는 것이다.

<br/>

> 이렇게 수행 중인 프로세스를 변경할 때, CPU의 레지스터 정보가 변경되는 것을 Context Switching이라고 한다.


<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/context-switching.png" width = 500/>


<br/>

## 문맥 교환(Context Switching)이란?
- CPU가 이전의 프로세스 상태를 PCB에 보관하고, 또 다른 프로세스의 정보를 PCB에 읽어 레지스터에 적재하는 과정을 말한다.

<br/>

### 문맥 교환은 언제 일어나는가?
- 인터럽트가 발생할 경우
- 실행 중인 CPU 사용 허가시간을 모두 소모할 경우
- 입출력을 위해 대기해야 하는 경우

<br/>

> 쉽게 말해, 프로세스가 상태를 변경할 경우 발생한다.
>
> 상태 변경의 예시는 아래와 같다.
>
> `Ready → Running`, `Running → Ready`, `Running → Waiting`

<br/>

### 문맥 교환 과정
- Task의 대부분 정보는 Register에 저장되고 PCB로 관리된다.
- 현재 실행하고 있는 Task의 PCB 정보를 저장한다.
- 다음 실행할 Task의 PCB 정보를 읽어 Register에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행할 수 있다. 

<br/>

---
사진 참조 : https://zitoc.com/context-switching