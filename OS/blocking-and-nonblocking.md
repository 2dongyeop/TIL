## 블로킹과 논블로킹
### 💡 블로킹(blocking) 방식
- 기존의 작업을 수행하던 중 필요한 값을 얻기 위해 **제어권을 넘겨주어** 작업 수행을 요청한다.
- 요청한 작업이 종료되면 결과값을 전달 받아, 기존의 작업을 이어서 수행한다.

<br/>

### 💡 논블로킹(non-blocking) 방식
- 기존의 작업을 수행하던 중 다른 작업 수행을 요청한 후, **제어권을 바로 돌려 받는다**.
- 작업 수행의 결과와는 상관없이 기존의 작업을 그대로 수행한다.

<br/>

## 그림으로 이해하기
### 💡 블로킹 방식
- 블로킹은 다른 함수를 호출하면, 제어권을 넘겨준다.

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/blocking.png" width = 500/>

<br/>

### 💡 논블로킹 방식
- 논블로킹은 다른 함수를 호출하고, 제어권을 자신이 그대로 가지고 있는다.(넘겨준 뒤 바로 반환)

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/non-blocking.png" width = 500/>
