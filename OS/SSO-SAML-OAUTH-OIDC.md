# SSO, SAML, OAuth, OIDC

## 💡 SSO

> SSO란?


`Single Sign-On` 의 약자로, **한 번의 로그인**으로 **다른 서비스들을 인증**할 수 있는 것이다.

다른 사이트에서 로그인 및 인증 부분만 따로 사용하는 것!

통합 인증, 단일 계정 로그인, 단일 인증이라고 한다.

<br/>

<br/>

> 예시


네이버 로그인을 통해 네이버 블로그, 네이버 클라우드 등을 이용할 수 있다!

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/15-1.png" width = 500/>

<br/>

<br/>

### SSO는 일반적으로 2가지 모델로 구분된다.

1. ***Delegation Medel***

사용자의 인증 정보를 Agent가 관리하여 대신 로그인해주는 방식이다.

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/15-2.png" width = 500/>


<br/>

<br/>

2. ***Propagation Model***

통합 인증을 수행하는 곳에서 인증을 받아 ‘인증 토큰’이란 것을 발급받는다.

사용자는 서비스에 접근할 때 발급받은 토큰을 이용해 서비스에 접근하는 방식!

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/15-3.png" width = 500/>


<br/>

<br/>

## 💡 SAML

> SAML이란?
> 

`Security Asserting Markup Language` 의 약자로,

네트워크를 통해 여러 컴퓨터에서 인증 및 권한 부여(SSO)를 하게 해주는 것이다.

XML을 이용해, cross domain 간 SSO 구현이 가능하다!

<br/>

<br/>


> 그림으로 이해하기
> 

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/15-4.png" width = 500/>

위 구성에서 SSO = IdP(Identify Provider), WEBSITE = SP(Service Provider)

***가정 1.  한 번도 로그인 하지 않은 경우***

1. 사용자가 SP를 사용하기 위해 접근
2. SP에 해당 사용자의 세션(정보)이 저장되어 있는지 체크
3. SP에 세션이 저장되어있지 않음 → IdP로 Redirect (= SSO 인증 모듈로 이동)
4. IdP에도 세션이 저장되어 있지 않아, 로긍니 페이지를 통해 사용자가 로그인하도록 한다.
5. 로그인 성공시 → IdP에 해당 사용자의 세션(정보)를 저장한 후 SP로 Redirect
6. SP에도 세션을 저장한 후 사용자가 SP를 사용할 수 있도록 한다.


<br/>

<br/>

***가정 2. 기존에 한 번 이상 로그인한 경우***

1. 사용자가 SP를 사용하기 위해 접근한다.
2. SP에 해당 사용자의 세션(정보) 이 저장되어있는지 체크한다.
3. SP에 세션이 저장되어 있지 않아, **IdP로 Redirect** 한다. (SSO 인증 모듈로 이동)

*3-1. SP에 세션이 저장되어 있는 경우 세션에 맞게 사용자가 SP를 사용할 수 있도록 한다.*

4. **IdP에 세션이 저장되어 있어,** **해당 세션을 가지고** **SP로 Redirect** 한다. (페이지 이동)
5. **SP에 IdP에서 가져온 세션을 저장한 후**(로그인 성공 전달) **사용자가 SP를 사용할 수 있도록** 한다.

<br/>

<br/>
    

### 인증 방식

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/15-5.png" width = 500/>

→ SAMLRequest & Response는 XML 형식이므로 브라우저를 통해서만 동작이 가능하다는 단점이 있다.

<br/>

<br/>

# 💡 OAuth

> OAuth란?
> 

사용자 **인증**을 위해 다른 앱 또는 웹사이트의 사용자 인증 방식으로 **인가**받는 프로토콜 → JSON 기반!

<br/>

해당 앱의 ID/PW로 직접 로그인하는 방식과는 범위의 제약이 존재한다.

→ 회사 직원의 출입증이 방문증을 받은 사람의 출입증보다 출입 가능 범위가 더 클 수 밖에 없다!

→ 실제 웹 사이트에서도 OAuth 사용자보다 직접 로그인한 사용자에게 여러 혜택을 제공하기도 한다.

<br/>
토큰 인증 방식을 사용한다..

<br/>

<br/>

### 알아야 할 개념들

1. ***Access Token***

OAuth의 핵심으로, 임의의 문자열 값을 가져 토큰을 발급해준 서비스만이 해당 토큰의 정보를 알고 있다.

토큰을 요청할 때 redirect_uri 값을 같이 요청하여 발급받을 위치를 지정한다.

⇒ 따라서 OAuth를 사용하는 Service Provider들은 AccessToken을 발급받을 위치를 등록해야 한다.

<br/>

<br/>

2. ***Refresh Token***

Access Token은 자신의 정보에 접근할 수 있게 하는 중요한 토큰이기에, 일정 유효 시간을 설정하곤 한다.

Refresh Token은 Access Token보다 유효기간이 길며, Access Token이 만료되어도 Refresh Token이 만료되지 않는 이상 로그인 없이 계속 발급받을 수 있다.


<br/>

<br/>

> 인증 방식

1. Authorization Code Grant (권한 코드 인증) 방식
    
<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/15-6.png" width = 500/>
    

<br/>

<br/>

2. Access Token이 만료되었을 경우

Client는 더 이상 Resource Server로부터 Resource를 가져올 수 없어 다시 토큰을 요청해야 한다.

<img src="https://github.com/2dongyeop/TIL/blob/main/OS/image/15-7.png" width = 500/>

<br/>

<br/>

3. Refresh Token이 만료되었을 경우

다시 로그인해야 한다.


<br/>

<br/>

# 💡 OIDC

OIDC(Open ID Connect)는 권한 허가 프로토콜인 OAuth 2.0을 이용해 만든 인증 레이어이다.

<br/>

<br/>

OAuth 

→ Authorization을 위한 기술, Access Token은 특정 리소스에 대한 권한을 부여받기 위한 메커니즘을 제공

<br/>

OIDC

→ Authentication(인증)을 위한 ID Token이라는 토큰을 이용해 OIDC를 수행

<br/>

<br/>

### ID Token

Access Token → Bearer 토큰 형식

ID Token → JWT(Json Web Token) 형식


<br/>

<br/>


JWT 구성 → Header, Payload, Signature 3부분

```json
{
  "iss": "<https://server.example.com>",
  "sub": "24400320",
  "aud": "s6BhdRkqt3",
  "exp": 1311281970,
  "iat": 1311280970
}
```