## 동기와 비동기
### 동기(synchronous: 동시에 일어나는) 방식
- 기존의 작업을 수행하던 중 다른 작업 수행을 요청한다.
- 요청한 작업의 **종료 여부를 확인한 후**, 종료 시 기존의 작업을 이어서 수행한다.
- 따라서 순서에 맞춰 진행되는 장점이 있지만,  여러 가지 요청을 동시에 처리할 수 없다.

<br/>

### 동기 방식의 장단점
- 장점 : 설계가 매우 간단하고 직관적이다.
- 단점 : 결과가 주어질 때까지 아무것도 못하고 대기해야 한다.

<br/>

### 비동기(asynchronous: 동시에 일어나지 않는) 방식
- 기존의 작업을 수행하던 중 다른 작업 수행을 요청한다.
    - 이때 작업 종료 후 **종료 여부를 판단할 동작도 같이 전달**한다.
- 요청한 작업의 종료 여부에 상관없이 기존의 작업을 이어서 수행한다.
- 따라서 여러 개의 요청을 동시에 처리할 수 있는 장점이 있지만 동기 방식보다 속도가 떨어질 수도 있다.

<br/>

### 비동기 방식의 장단점
- 장점 : 결과가 주어지는데 시간이 걸리더라도, 그 시간 동안 작업을 할 수 있으므로 자원을 효율적으로 사용한다.
- 단점 : 동기 방식보다 복잡하다.

<br/>

## 그림으로 이해하기
#### 동기(Synchronous) 방식
- 손님은 알바생이 신발을 가져오는 것을 계속해서 확인한다.

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/sync.png" width = 500/>


<br/>

#### 비동기(Asynchronous) 방식
- 손님은 알바생이 신발을 가져오는 것을 신경쓰지 않는다.
- 작업이 종료되면 신발을 가져올 것(종료 여부를 판단할 동작)을 알기 때문이다.

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/async.png" width = 500/>



<br/>

---
#### References
- [NKLCWDT님](https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%EB%8F%99%EA%B8%B0_%EB%B9%84%EB%8F%99%EA%B8%B0_%EB%B8%94%EB%A1%9C%ED%82%B9_%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9.md)
- [dev-coco님](https://dev-coco.tistory.com/46)