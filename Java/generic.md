# 제네릭

- 본 내용은 [이것이 자바다 - 신용권]을 참고하여 정리하였습니다.
- [소스 코드 레퍼지토리](https://github.com/2dongyeop/thisisjava)로 이동하기

<br/>

> 1절. 왜 제네릭을 사용해야 하는가?
> 
> 2절. 제네릭 타입(class<T>, interface<T>)
>
> 3절. 멀티 타입 파라미터(class<K,V, ...>, interface<K,T, ...>)
> 
> 4절. 제네릭 메소드(<T,R> R method(T t))
>
> 5절. 제한된 타입 파라미터(<T extends 최상위타입>)
>
> 6절. 와일드카드 타입(<?>, <? extends ...>, <? super>)
> 
> 7절. 제네릭 타입의 상속과 구현

<br/>

## 1. 왜 제네릭을 사용해야 하는가?
### 이유
- `제네릭(Generic)` 타입을 이용하면 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거할 수 있게 되었다.
- 제네릭은 클래스와 인터페이스, 그리고 메소드를 정의할 때 타입을 파라미터로 사용할 수 있도록 한다. 
- 타입 파라미터는 코드 작성 시 구체적인 타입으로 대체되어 다양한 코드를 생성하도록 해준다.

<br/>

> 제네릭을 사용할 경우 아래와 같은 이점이 있다.
> 1. 컴파일 시 강한 타입 체크를 할 수 있다.
>    - 실행 시 타입 에러가 나는 것보다는 컴파일 시에 미리 타입을 강하게 체크해서 에러를 사전에 방지하는 것이 좋다.
> 2. 타입 변환(casting)을 제거한다.
>    -  비제네릭 코드는 불필요한 타입 변환을 하기 때문에 프로그램 성능에 악영향을 미친다.  

<br/>

## 2. 제네릭 타입(class<T>, interface<T>)

```java
public class 클래스명<T> {...}
public interface 인터페이스명<T> {...}
```
- 제네릭 타입은 타입을 파라미터로 가지는 클래스와 인터페이스를 말한다. 

<br/>

### 사용 방법 비교
- Object 클래스 사용
```java
public class Box {
	private Object object;
	public void set(Object object) { this.object = object; }
	public Object get() { return object; }
}
```
- 필드 타입을 `Object` 타입으로 선언한 이유는 필드에 모든 종류의 객체를 저장하기 위함이다.
    - `Object` 클래스는 모든 자바 클래스의 최상위 조상(부모) 클래스이다.
    - 따라서 자식 객체는 부모 타입에 대입할 수 있다는 성질 때문에 **모든 자바 객체는 Object 타입으로 자동 타입 변환되어 저장된다.**

<br/>

> 이처럼 Object 클래스를 사용하면 모든 종류의 객체를 저장할 수 있다는 장점은 있지만, 저장할 때와 읽어올 때 타입 변환이 발생하고, 이러한 타입 변환이 빈번해지면 전체 프로그램 성능에 좋지 못한 결과를 가져올 수 있다.


<br/>

- 제네릭 사용
```java
public class Box<T> {
	private T t;
	public T get() { return t; }
	public void set(T t) { this.t = t; }
}
```
- 타입 파라미터는 일반적으로 대문자 알파벳 한 글자로 표현한다. 
- 제네릭 타입은 클래스 또는 인터페이스 이름 뒤에 “<>”부호가 붙는다.
    - 그 사이에 타입 파라미터가 위치한다.
- 이로써 모든 종류의 객체를 저장하면서 타입 변환이 발생하지 않도록 하였다.


<br/>

## 3. 멀티 타입 파라미터
### 사용 예시
```java
public class Product <T,M> {
    private T kind;
    private M model;
    
    public T getKind() { return this.kind; }
    public M getModel() { return this.model; }
    
    public void setKind(T kind) { this.kind = kind;}
    public void setModel(M model) { this.model = model; }
}
```

- 제네릭 타입은 두 개 이상의 멀티 타입 파라미터를 사용할 수 있는데, 이 경우 콤마로 구분한다.

<br/>

## 4. 제네릭 메소드
### 정의
- 제네릭 메소드는 매개 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드를 말한다.

<br/>

### 작성 방법
```java
public <타입파라미터, ...> 리턴타입 메소드명(매개변수, ...) {...}
pubiic <T> Box<T> boxing(T t) {...}
```

<br/>

### 호출 방법
```java
리턴타입 변수 = <구체적타입> 메소드명(매개값);  
리턴타입 변수 = 메소드명(매개값);              
```
1. 코드에서 구체적인 타입을 명시적으로 지정
2. 컴파일러가 매개값의 타입을 보고 구체적인 타입을 추정

<br/>

## 5. 제한된 타입 파라미터
### 필요 예시
- 타입 파라미터에 지정되는 구체적인 타입을 제한할 필요가 종종 있다. 
- 예를 들어 숫자를 연산하는 제네릭 메소드는 매개값으로 `Number` 타입 또는 하위 클래스 타입(Byte, Short, Integer 등)의 인스턴스만 가져야 한다.
- 이것이 제한된 타입 파라미터(bounded type parameter)가 필요한 이유다.

<br/>

### 작성 방법
```java
public <T extends 상위타입> 리턴타입 메소드(매개변수, ...) {...}
```
- 제한된 타입 파라미터를 선언하려면 타입 파라미터 뒤에 extends 키워드를 붙이고 상위 타입을 명시한다. 
- 상위 타입은 클래스 뿐만 아니라 인터페이스도 가능하다. 
    - 단, 인터페이스라고 해서 implements를 사용하진 않는다.
- 타입 파라미터에 지정되는 구체적인 타입은 상위 타입이거나 상위 타입의 하위 또는 구현 클래스만 가능하다. 
    - **주의할 점은 메소드의 중괄호 { } 안에서 타입 파라미터 변수로 사용 가능한 것은 상위 타입의 멤버(필드, 메소드)로 제한되며, 하위 타입에만 있는 필드와 메소드는 사용할 수 없다.**

<br/>

## 6. 와일드카드 타입
### 와일드카드
- 코드에서 `?`를 일반적으로 와일드카드(wildcard)라고 부른다.

<br/>

### 활용 방법
> Person 클래스를 상속 받는 Worker와 Student 클래스가 있다고 가정한다.
1. 제네릭 타입<?> : Unbounded Wildcards (제한 없음)
    - 타입 파라미터를 대치하는 구체적인 타입으로 모든 클래스나 인터페이스 타입이 올 수 있다.
        - Course<?> → 수강생은 모든 타입이 될 수 있다.
2. 제네릭 타입<? extends 상위타입> : Upper Bounded Wildcards (상위 클래스 제한)
    - 타입 파라미터를 대치하는 구체적인 타입으로 하위 타입만 올 수 있다.
        - Course<? extends Student> → 수강생은 Student와 HighStudent만 될 수 있다.
3. 제네릭 타입<? super 하위타입> : Lower Bounded Wildcards (하위 클래스 제한)
    - 타입 파라미터를 대치하는 구체적인 타입으로 상위 타입이 올 수 있다.
        - Course<? super Worker> → 수강생은 Worker와 Person만 될 수 있다.

<br/>

## 7. 제네릭 타입의 상속과 구현
### 사용 예시
```java
public class ChildProduct<T, M> extends Product<T, M> {...}
```
- 제네릭 타입도 다른 타입과 마찬가지로 부모 클래스가 될 수 있다.

<br/>

```java
public class ChildProduct<T, M, C> extends Product<T, M> {...}
```
- 자식 제네릭 타입은 추가적으로 타입 파라미터를 가질 수 있다.

<br/>

## 마무리
글을 간략하게 작성하다보니 생략된 코드들이 있으니, 자세히 보려면 [링크](https://github.com/2dongyeop/thisisjava/tree/main/src/oldCode/exerciseCode/chapter13)를 클릭해주세요.