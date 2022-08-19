## 추상 클래스(abstract class)
- abstract 키워드로 선언된 클래스
- 추상 메서드를 최소 한 개 이상 가진다.

<br/>

### 추상 클래스의 구현 방법
- 서브 클래스에서 슈퍼 클래스의 모든 추상 메서드를 오버라이딩하여 실행가능한 코드로 구현한다.

<br/>

### 추상 클래스의 사용 목적
- 객체(인스턴스)를 생성하기 위함이 아니며, 상속을 위한 부모 클래스로 활용하기 위한 것이다.
- 여러 클래스들의 공통된 부분을 추상화(추상 메서드) 하여 상속받는 클래스에게 구현을 강제화하기 위한 것이다. 
    - 메서드의 동작을 구현하는 자식 클래스로 책임을 위임
- 즉, 추상 클래스의 추상 메서드를 자식 클래스가 구체화하여 그 기능을 확장하는 데 목적이 있다.


<br/>

### 추상 클래스 사용 예시
```java
abstract class Shape { // 추상 클래스
    Shape() {...}
    void edit() {...}
    abstract public void draw(); // 추상 메서드
}
```

```java
/* 추상 클래스의 구현 */
class Circle extends Shape {
    public void draw() { System.out.println("Circle"); } 
    void show() { System.out.println("동그라미 모양"); }
}
```

<br/>

## 인터페이스
- 추상 메서드와 상수만을 포함하며, interface 키워드를 사용하여 선언한다.

<br/>

### 인터페이스 구현 방법
- 인터페이스를 상속받고, 추상 메서드를 모두 구현한 클래스를 작성한다.
- implements 키워드를 사용하여 구현한다.

<br/>

### 인터페이스의 사용 목적
- 상속받을 서브 클래스에게 구현할 메서드들의 원형을 모두 알려주어, 클래스가 자신의 목적에 맞게 메서드를 구현하도록 하는 것이다.
- 구현 객체의 같은 동작을 보장하기 위한 목적이 있다.
- 즉, 서로 관련이 없는 클래스에서 공통적으로 사용하는 방식이 필요하지만 기능을 각각 구현할 필요가 있는 경우에 사용한다.

<br/>

### 인터페이스 사용 예시
```java
interface Phone { // 인터페이스
  final int BUTTONS = 20; // 상수 필드 
  void sendCall(); // 추상 메서드 - abstract public void
  abstract public void receiveCall(); // 추상 메서드
}
```

```java
class FeaturePhone implements Phone {
  // Phone의 모든 추상 메서드를 구현한다.
  public void sendCall() {...}
  public void receiveCall() {...}

  // 추가적으로 다른 메서드를 작성할 수 있다.
  public int getButtons() {...}
}
```

<br/>

## 추상 클래스와 인터페이스의 공통점
- 인스턴스(객체)는 생성할 수 없다.
- 선언만 있고 구현 내용이 없다.
- 자식 클래스가 메서드의 구체적인 동작을 구현하도록 책임을 위임한다.

<br/>

## 추상 클래스와 인터페이스의 차이점
- 서로 다른 목적을 가지고 있다.
  - 추상 클래스는 추상 메서드를 자식 클래스가 구체화하여 그 기능을 확장하는 데 목적이 있다. (상속을 위한 부모 클래스)
  - 인터페이스는 서로 관련이 없는 클래스에서 공통적으로 사용하는 방식이 필요하지만 기능을 각각 구현할 필요가 있는 경우에 사용한다. (구현 객체의 같은 동작을 보장)
- 추상 클래스는 단일 상속이지만 인터페이스는 다중 상속이 가능하다.
- 추상 클래스는 “is a kind of” 인터페이스는 “can do this”
  - Ex) 추상 클래스: Appliances(Abstract Class) - TV, Refrigerator
  - Ex) 인터페이스: Flyable(Interface) - Plane, Bird