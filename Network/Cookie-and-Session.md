## HTTP의 Stateless, Connectionless, 쿠키와 세션, 그리고 JWT까지!

<br/>

### 💡 HTTP는 두 가지 성질을 가진다.
- `Connectionless`(비연결성)
  - 클라이언트가 request를 서버에 보내고, 서버가 클라이언트의 요처에 맞는 response를 보내면 바로 연결을 끊는다!
- `Stateless`(무상태)
  - 연결을 끊는 순간 클라이언트와 서버의 통신은 끝나며 상태 정보를 유지하지 않는다.


<br/>

<br/>

### 💡 자연스럽게 제시되는 쿠키와 세션의 필요성
- HTTP 프로토콜은 위와 같은 특징으로 모든 요청 간에 의존관계가 없다.
  - 즉, 현재 접속한 사용자가 이전에 접속했는지 아닌지 알 수 있는 방법이 없다!
  - → 이는 계속 연결을 유지하지 않기 때문에 리소스 낭비가 줄어드는 것이 큰 장점이지만,
  - → 통신할 때마다 새로 연결해야 하므로 요청할 때마다 인증을 해야 한다는 단점으로도 다가온다.
- HTTP 프로토콜에서는 쿠키와 세션을 사용해 이전 요청과 현재 요청이 같은 사용자의 요청인지를 판단한다.


<br/>

<br/>

### 💡 쿠키란?
클라이언트 로컬에 저장되는 키와 값이 들어있는 파일이다.
- 이름, 값, 유효시간, 경로 등을 포함한다.
- 클라이언트의 상태 정보를 브라우저에 저장하여 참조한다.

<br/>

- 그림으로 보는 동작방식
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/cookie1.png" width = 500/>
    <img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/cookie2.png" width = 500/>


<br/>

<br/>

### 💡 세션이란?
일정시간 동안 같은 브라우저로부터 들어오는 요청을 하나의 상태로 보고 그 상태를 유지하는 기술이다.
- 즉, 웹 브라우저를 통해 접속한 서버 이후부터 브라우저를 종료할 때까지 유지되는 상태이다.

<br/>

- 그림으로 보는 동작방식

    <img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/session1.png" width = 500/>

<br/>

> *세션도 쿠키를 사용하여 값을 주고 받으며 클라이언트의 상태 정보를 유지한다.*
>
> → 즉, 상태 정보를 유지하는 수단은 **쿠키**이다!


<br/>

<br/>

### 💡 쿠키와 세션의 차이점
- 저장 위치
  - 쿠키 : 클라이언트
  - 세션 : 서버
- 보안
  - 쿠키 : 클라이언트에 저장되므로 보안에 취약하다.
  - 세션 : 쿠키를 이용해 Session ID만 저장하고, 이 값으로 구분해서 서버에서 처리하므로 비교적 보안성이 좋다.
- 라이프 사이클
  - 쿠키 : 만료시간에 따라 브라우저를 종료해도 계속해서 남아있을 수 있다.
  - 세션 : 만료시간을 정할 수 있지만 브라우저가 종료되면 만료시간에 상관없이 삭제된다.
- 속도
  - 쿠키 : 클라이언트에 저장되어서 서버에 요청 시 빠르다.
  - 세션 : 실제 저장된 정보가 서버에 있으므로, 서버의 처리가 필요해 쿠키보다 느리다.

<br/>

<br/>

### 💡 JWT란?
JSON Web Token의 약자로, 인증에 필요한 정보들을 암호화 시킨 토큰이다.

<br/>

- JWT의 구조
  - `Header` : 토큰의 타입과 JWT를 서명하는 데 사용한 알고리즘을 명시한다.
  - `Payload` : 유저의 정보를 의미한다.
    - 이때 정보의 한 조각을 클레임(Claim)이라 부르며, name과 value의 쌍으로 이루어진다.
  - `Signature` : 서버에서 토큰의 정보가 서버로부터 생성된 것인지 증명하기 위해 사용
    
    <img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/jwt1.png" width = 500/>

<br/>

- 그림으로 보는 동작방식
  
  <img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/jwt2.png" width = 500/>
  <img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/jwt3.png" width = 500/>

    1. 유저가 자신의 ID/PW를 입력해 로그인을 한다.
    2. Authentication 서버는 credential을 검증하고, 정보가 맞다면 서명된 JWT를 발급한다.
    3. 유저(클라이언트)는 서버에게 자신의 정보 자원에 접근하기 위해 JWT를 HTTP Authorization Header에 담아 보낸다.
    4. 서버는 토큰의 authenticity를 public key나 secret을 활용해 토큰의 authenticity를 검증한다.

<br/>

- JWT의 장단점
  - 장점
    1. Header와 Payload를 가지고 Signature를 생성하므로 데이터 위변조를 막을 수 있다.
    2. 인증 정보에 대한 별도의 저장소가 필요없다.
    3. JWT는 토큰에 대한 기본정보와 전달할 정보 및 토큰이 검증됬음을 증명하는 서명 등 필요한 모든 정보를 자체적으로 지니고 있다.
  
  <br/>

  - 단점
    1. 쿠키/세션과 다르게 JWT는 토큰의 길이가 길어, 인증 요청이 많아질수록 네트워크 부하가 심해진다.
    2. Payload 자체는 암호화 되지 않기 때문에 유저의 중요한 정보는 담을 수 없다.
    3. 토큰은 한번 발급 되면 유효기간이 만료될 때 계속 사용되어 탈취 당하게 되면 대처하기 힘들다.

