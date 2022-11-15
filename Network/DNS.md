## DNS 주소가 IP 주소로 바뀌는 과정
→ feat. 주소창에 `naver.com`을 입력했을 때 일어나는 일

<br/>

<br/>

### 필요한 사전 지식
#### 1. IP address
-  Internet Protocol을 의미하며, 인터넷에 연결되어 있는 장치들에게 부여되는 고유한 주소이다.
-  총 32비트로 되어 있으며 네트워크 계층에서 동작한다.
- ex) 127.0.0.1

<br>

#### 2. DNS
- Domain Name System의 약자로, 주소를 기억하기 쉽게 이름을 정한 것이다.
- IP 주소가 숫자로 이루어져 있어 사람이 구별하고 외우기가 어려운 문제를 보완하였다.
- ex) naver.com

<br/>

#### 3. 도메인 이름 체계

<img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/DNS-name.png" width = 600/>

<br/>

<br/>

### 도메인 주소가 IP 주소로 바뀌는 과정 (feat. 주소 창에 `naver.com`을 입력하면 일어나는 일)

<img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/DNS-process.png" width = 600/>

1. 주소창에 도메인 주소가 입력된다.

    → 도메인 주소가 Local DNS에 전달된다.
2. Local DNS는 Root DNS에 도메인 주소를 보내고 이에 맞는 IP 주소를 요청하게 된다.

3. Root DNS에서 도메인을 관리하는 서버에 대한 정보를 받는다.
    
    → Root DNS는 IP 주소를 저장하는 것이 아니라, `.com` DNS 서버에 대한 정보만 가지고 있다.

    → 따라서 해당 `.com` DNS 서버에 대한 정보를 Local DNS에 보낸다.

4. `.com` DNS 서버로 접근해 다시 도메인 주소를 보낸다.
5. `.com` DNS 서버는 진짜 IP 주소를 가진 네임 서버의 정보를 반환한다.
6. 진짜 IP 주소를 가진 네임 서버에 도메인 주소를 보내고 IP 주소를 요청한다.
7. 네임 서버로부터 IP 주소를 받는다.
8. IP 주소 정보가 실제 컴퓨터에 응답된다.

<br/>

<br/>

### 참고자료
- [한국인터넷진흥원](https://www.kisa.or.kr/1050403)
- [aws.amazon.com](https://aws.amazon.com/ko/route53/what-is-dns/)
- [Formagran님](https://fomaios.tistory.com/entry/Network-DNS%EB%9E%80-feat-DNS-%EA%B3%BC%EC%A0%95-What-is-Domain-Name-System)
