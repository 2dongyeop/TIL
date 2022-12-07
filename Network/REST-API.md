# REST API

<br/>

## 💡 REST(Representational State Transfer)
- 자원(resource)을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 말한다.
- 자원의 표현에 의한 상태를 전달한다.
  - 자원 : 해당 소프트웨어가 관리하는 모든 것
  - 자원의 표현 : 그 자원을 표현하기 위한 이름
    - ex) DB에서 영화 정보가 자원이라면? → 'movies'와 같이 표현한다.
  - 상태 전달
    - 데이터가 요청되어지는 시점에서 자원의 상태를 전달한다.
    - 주로 JSON 혹은 XML을 통해 데이터를 주고 받는다.

<br/>

<br/>

## 💡 REST 서브 개념
- REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용한다.
  - **웹의 장점을 최대한 활용할 수 있다.**
- HTTP Method(`GET`, `POST`, `PUT`, `PATCH`, `DELETE`)를 통해 해당 자원에 대한 CRUD Operation을 적용한다.
  - CRUD Operation
    - Create : 생성(POST)
    - Read : 조회(GET)
    - Update : 수정(PUT)
    - Delete : 삭제(DELETE)
    - HEAD : header 정보 조회(HEAD)
  - 이에 대한 내용을 [정리한 링크](https://github.com/2dongyeop/TIL/blob/main/Network/http-method.md)를 첨부한다.

<br/>

<br/>


## 💡 REST의 장단점
### 장점
- REST의 장단점
장점
- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- **REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.**

<br/>

### 단점
- 표준이 존재하지 않는다.
- 사용할 수 있는 메소드가 4가지 밖에 없다.
  - HTTP Method 형태가 제한적이다.
- 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재한다.
  - PUT, DELETE를 사용하지 못하는 점

<br/>

<br/>

## 💡 REST 구성 요소
- 자원(Resource): URI
  - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
  - 자원을 구별하는 ID는 ‘`/groups/:group_id`’와 같은 HTTP URI 다.
  - Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.
- 행위(Verb): HTTP Method
  - HTTP 프로토콜의 Method를 사용한다.
  - HTTP 프로토콜은 `GET`, `POST`, `PUT`, `DELETE`와 같은 메서드를 제공한다.
- 표현(Representation of Resource)
  - Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
  - JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.

<br/>

<br/>

> 그래서 REST는 대충 알겠고, REST API가 뭔데?

<br/>

<br/>

## 💡 REST API(Application Programming Interface)
- REST 기반으로 서비스 API를 구현한 것
- 사내 시스템들도 REST 기반으로 시스템을 분산해 **확장성과 재사용성을 높여** 유지보수 및 운용을 편리하게 할 수 있다.
- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.

<br/>

<br/>

## 💡 REST API 설계 기본 규칙

> 사전에 알아야 할 개념
> - 도큐먼트 : 객체 인스턴스나 데이터베이스 레코드와 유사한 개념
> - 컬렉션 : 서버에서 관리하는 디렉터리라는 리소스
> - 스토어 : 클라이언트에서 관리하는 리소스 저장소

<br/>

- URI는 정보의 자원을 표현해야 한다.
  - resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
  - resource의 도큐먼트 이름으로는 단수 명사를 사용해야 한다.
  - resource의 컬렉션 이름으로는 복수 명사를 사용해야 한다.
  - resource의 스토어 이름으로는 복수 명사를 사용해야 한다.
    - Ex) `GET /Member/1` → `GET /members/1`

<br/>

- 자원에 대한 행위는 HTTP Method(`GET`, `PUT`, `POST`, `DELETE` 등)로 표현한다.
  - URI에 HTTP Method가 들어가면 안된다.
    - Ex) `GET /members/delete/1` → `DELETE /members/1`
  - URI에 행위에 대한 동사 표현이 들어가면 안된다.(즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.)
    - Ex) `GET /members/show/1` → `GET /members/1`
    - Ex) `GET /members/insert/2` → `POST /members/2`
  - 경로 부분 중 변하는 부분은 유일한 값으로 대체한다. ( → 즉, `:id`는 하나의 특정 resource를 나타내는 고유값이다.)
    - Ex) student를 생성하는 route: `POST /students`
    - Ex) id=12인 student를 삭제하는 route: `DELETE /students/12`

<br/>

<br/>

## 💡 REST API URI 규칙
- 슬래시 구분자('`/`')는 계층 관계를 나타내는데 사용한다.
  - Ex) `http://restapi.example.com/houses/apartments`
- URI 마지막 문자로 슬래시('`/`')를 포함하지 않는다.
  - Ex) `http://restapi.example.com/houses/apartments/` (X)
- 하이픈('`-`')은 URI 가독성을 높이는데 사용한다.
  - 불가피하게 긴 URI경로를 사용하게 된다면 하이픈을 사용해 가독성을 높인다.
- 밑줄('`_`')은 URI에 사용하지 않는다.
  - 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용하지 않는다.
- URI 경로에는 소문자가 적합하다.
  - URI 경로에 대문자 사용은 피하도록 한다.
- 파일확장자는 URI에 포함하지 않는다.
  - Ex) `http://restapi.example.com/members/soccer/345/photo.jpg` (X)

<br/>

<br/>

## 💡 REST API 설계 예시

|CRUD|HTTP verbs|Route|
|:---|:---|:---|
|resource들의 목록을 표시|`GET`|`/resource`|
|resource 하나의 내용을 표시|`GET`|`/resource/:id`|
|resource를 생성|`POST`|`/resource`|
|resource를 수정|`PATCH`|`/resource/:id`|
|resource를 수정|`PUT`|`/resource/:id`|
|resource를 삭제|`DELETE`|`/resource/:id`|