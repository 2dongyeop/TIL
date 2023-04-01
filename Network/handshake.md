# 3-way handshake & 4-way handshake

### 💡 *3-way handshake란?*
- TCP에서 정확한 전송을 보장하는 **연결을 성립**하는 과정이다.
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하기 위함이다.

<br/>

<br/>

### 💡 *4-way handshake란?*

- TCP의 **연결을 해제**하는 과정이다.

<br/>

<br/>

### 💡 3 way handshake 동작 과정

<img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/handshake1.png" width = 900/>

1. 클라이언트가 서버에서 SYN 패킷을 보낸다.
2. 서버가 SYN 패킷을 받고, 받았다는 신호인 ACK와 함께 SYN 패킷을 보낸다.
3. 클라이언트는 ACK 응답과 SYN 패킷을 받고, 서버가 보낸 SYN에 대한 ACK를 보낸다.

<br/>

<br/>

### 💡 4 way handshake 동작 과정

> *연결 성립 후, 모든 통신이 끝났다면 해제 과정이 필요하다.*


<img src="https://github.com/2dongyeop/TIL/blob/main/Network/image/handshake1.png" width = 900/>
 
1. 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.
2. 서버는 FIN을 받고, 확인했다는 ACK를 보낸다.
3. 데이터를 모두 보냈다면, 서버는 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보낸다.
4. 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다.
5. 서버는 ACK를 받은 후 소켓을 닫는다.
6. 일정 시간이 지나면 클라이언트도 소켓을 닫는다.

<br/>

<br/>

### 💡 연결 과정은 3단계인데, 왜 종료 과정은 4단계일까?

> 클라이언트가 데이터 전송을 마쳤다고 하더라도, 서버는 아직 보낼 데이터가 남아있을 수 있다.
> 
> 
> 따라서 서버가 일단 FIN에 대한 ACK만 보내고, 데이터를 모두 전송한 후에 자신도 FIN 메시지를 보내기 때문!
>