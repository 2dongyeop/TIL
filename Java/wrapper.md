## Wrapper 클래스
- 자바의 자료형은 크게 기본 타입(primitive type)과 참조 타입(reference type)으로 나뉜다.

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/wrapper1.png" width = 500/>

- 자바는 모든 기본 타입은 값을 갖는 객체를 생성 할 수 있다.
- 단, 래퍼 클래스로 감싸고 있는 기본 타입 값은 외부에서 변경할 수 없다.

<br/>

<br/>

### Wrapper 클래스 구조

<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/wrapper2.png" width = 500/>

- 모든 래퍼 클래스의 부모는 Object 클래스이고, 숫자를 다루는 래퍼클래스의 부모는 Number이다.

<br/>

<br/>

### 박싱과 언박싱

- 박싱(Boxing) : 기본 타입의 값을 포장 객체로 만드는 과정
    
    ```java
    Integer obj = new Integer(100);      //포장 클래스의 생성자로 박싱
    Integer obj = Integer.valueof(100);  //정적 메소드를 이용하여 박싱
    ```
    
    <br/>

- 언박싱(UnBoxing) : 포장 객체에서 기본 타입의 값을 얻어내는 과정
    
    ```java
    int num = obj.intValue();
    ```

<br/>

<br/>

### 용도

- 포장 클래스는 문자열을 기본 타입의 값으로 변환할 때 많이 사용된다.
    - `parse + 기본타입` 명으로 있는 정적 메소드를 이용해 언박싱 할 수 있다.
    
        ```java
        String str = "10";
        
        int num = Integer.parseInt(str);
        ```
    
<br/>

## 마무리

- 기본 데이터 타입을 Object 타입으로 변환할 수 있다.
- 컬렉션 프레임워크의 데이터 구조는 기본 타입이 아닌 객체만 저장하게 되고, Wrapper 클래스를 이용해 자동 박싱 / 언박싱을 한다.