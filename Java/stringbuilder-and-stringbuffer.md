## String
- 새로운 값을 할당할 때마다 객체가 생성된다.
    - 문자열 연산시 새로 객체를 만드는 Overhead 발생
- String에서 저장되는 문자열은 `private final char[]` 의 형태라, 값을 바꿀 수 없다. → `Immutable`
    - 객체가 불변하므로, multi-thread에서 동기화를 신경 쓸 필요가 없음.
- String + String + String
    - 각각의 String 주소 값이 스택에 저장되고, GC가 호출되기 전까지는 생성된 String 객체들은 힙에 관리된다. → 메모리 관리에 치명적
    - String을 직접 더하는 것 보다는 StringBuffer나 StringBuilder를 사용하는 것이 좋다.

<br/>

## StringBuilder & StringBuffer
- 메모리에 append하는 방식으로, 클래스에 대한 객체를 직접 생성하지 않는다.

<br/>

**StringBuilder**

- 변경 가능한 문자열
- 비동기 처리 방식

**StringBuffer**

- 변경 가능한 문자열
- 동기 처리 방식
    - multi-thread 환경에서 안전하다.

<br/>

> IntelliJ에서 내부 코드를 보면 StringBuffer의 경우 동기화를 위해 `synchronized`  키워드가 붙은 것을 볼 수 있다.

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/StringBuilder-StringBuffer.png" width = 900/>