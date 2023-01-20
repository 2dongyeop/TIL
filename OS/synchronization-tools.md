# 동기화 도구들

<br/>

> 🌱 *협력적 프로세스란?*
>
>→ 시스템 내에서 실행 중인 다른 프로세스의 실행에 영향을 주거나 받는 프로세스!
>
>→ 논리 주소 공간을 공유하거나, 공유 메모리 또는 메시지 전달을 통해 데이터를 공유한다.

<br/>

<br/>

## 배경 

> 프로세스가 병행 또는 병렬로 실행될 때 **공유 데이터의 무결성**에 어떤 문제를 일으키는지에 관해 알아보자!
> 

<br/>

<br/>

- 유한 버퍼 문제로 가정해보자.
    - 동시에 최대 `BUFFER_SIZE - 1` 개 까지만 버퍼에 저장할 수 있다.

<br/>

<br/>

- 첫번째 방법 : `count` 에 버퍼에 있는 개수를 저장하자.
    - 생산자 코드
        
        ```java
        while (true) {
        		
        		while (count == BUFFER_SIZE) ;
        
        		buffer[in] = next_produced;
        		in = (in + 1) % BUFFER_SIZE;
        		count++;
        }
        ```
    
    <br/>
    
    - 소비자 코드
        
        ```java
        while (true) {
        
        		while (count == 0) ;
        
        		next_consumed = buffer[out];
        		out = (out + 1) % BUFFER_SIZE;
        		count--;
        }
        ```
        

<br/>

<br/>

- 결과
  - 첫번째 방식은 병행으로 실행 시 올바르게 동작하지 않는다.
  
    <img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/buffer-count.png" width = 600/>

<br/>

<br/>


> 🌱 **경쟁 상황**(race condition)이란?
>
>→ 여러 프로세스가 동일한 자료에 접근하여 조작하고, 그 실행 결과가 접근이 발생한 특정 순서에 의존하는 상황을 말한다. 이를 피하기 위해 **동기화**는 필수적이다.

<br/>

<br/>

