## HTTP methods
이번 글에서는 postman을 이용하여 서버에 요청을 보내는 과정에서 알게 된 http 메소드에 대해 다루었다.

<br/>


### HTTP 메소드란?

- <span style="color:orange">**클라이언트가 웹 서버에게 사용자 요청의 목적이나 종류를 알리는 수단**</span>으로, 9가지가 있다.

<br/>

---
### GET

- 주로 <span style="color:orange">**리소스를 조회**</span>할 때 사용하며, 서버에 전달하고 싶은 데이터는 query를 통해 전달한다.
- 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 있어 권장하지 않는다.

### POST

- 데이터 요청을 처리하고, 메시지 바디를 통해 서버로 데이터를 전달한다.
- 주로 <span style="color:orange">**신규 리소스를 등록**</span>하거나, 프로세스 처리에 사용된다.

### PUT

- <span style="color:orange">**리소스가 있으면 대체**</span>하고, 리소스가 없으면 생성한다.
- 쉽게 말해 데이터를 덮어쓴다

### PATCH

- PUT과 마찬가지로 <span style="color:orange">**리소스를 수정**</span>할 때 사용한다.
- 단, PATCH는 리소스를 일부분만 변경할 수 있는 차이점이 있다.

### DELETE

- <span style="color:orange">**리소스를 제거**</span>할 때 사용한다.

<br/>

> 이외에도 `HEAD`, `OPTIONS`, `CONNECT`, `TRACE` 등의 메소드가 있고, [링크](https://kyun2da.dev/CS/http-%EB%A9%94%EC%86%8C%EB%93%9C%EC%99%80-%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C/)를 통해 찾아 볼 수 있다.



<br/>

### HTTP 메소드의 속성

- `안전(Safe Methods)` : 이 말을 <span style="color:orange">**메소드를 호출해도 리소스를 변경하지 않는다**</span>는 뜻이다.
    - 주요 메소드 중에는 `GET` 메소드가 안전하다고 할 수 있다.
- `멱등(Idempotent Methods)` : 이 말은 <span style="color:orange">**메소드를 계속 호출해도 결과가 똑같다**</span>는 뜻이다.
    - `GET`, `PUT`, `DELETE` 메소드가 멱등하다고 할 수 있다.
    - `POST`, `PATCH` 메소드는 멱등하다고 볼 수 없다.
- `캐시 가능(Cacheable Methods)` : <span style="color:orange">**캐싱을 해서 데이터를 효율적으로 가져올 수 있다**</span>는 뜻이다.
    - `GET`, `HEAD`, `POST`, `PATCH`가 캐시가 가능하지만, 실제론 `GET` 과 `HEAD`만 주로 캐싱이 쓰인다.
    
---
    
- 이외에도 여러 특징들을 아래 사진과 같이 정리할 수 있다.

<img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/http-method.png" width = 500/>
