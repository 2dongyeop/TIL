# 컬렉션 프레임워크

- 본 내용은 [이것이 자바다 - 신용권]을 참고하여 정리하였습니다.
- [소스 코드 레퍼지토리](https://github.com/2dongyeop/thisisjava)로 이동하기

<br/>

> 1절. 컬렉션 프레임워크 소개 
> 
> 2절. List 컬렉션
>
> 3절. Set 컬렉션
> 
> 4절. Map 컬렉션
>
> 5절. 검색 기능을 강화시킨 컬렉션
>
> 6절. LIFO와 FIFO
> 
> 7절. 동기화된 컬렉션
> 
> 8절. 병렬 처리를 위한 컬렉션

<br/>


## 1. 컬렉션 프레임워크 소개
### 개요
자바는 배열의 문제점을 널리 알려져 있는 `자료구조`를 바탕으로 객체들을 효율적으로 추가, 삭제, 검색할 수 있도록 `java.util` 패키지에 컬렉션과 관련된 인터페이스들을 포함시켰고, 이를 `컬렉션 프레임워크`라고 한다.

<br/>

### 컬렉션
- 사전적 의미로는 요소를 수집해서 저장하는 것을 말한다.
- 자바 컬렉션은 객체를 수집해서 저장하는 역할을 한다.

<br/>

### 프레임워크 
- 사용 방법을 미리 정해 놓은 라이브러리를 말한다.

<br/>

> 아래는 해당 인터페이스를 구현한 클래스이다.

|인터페이스 분류|    특징 | 구현 클래스|
|:-------:|:------|:-------|
|Collection - <br/>List 계열|순서를 유지하고 저장 <br/>중복 저장 가능 | ArrayList, Vector, LinkedList|
|Collection - <br/>Set 계열|순서를 유지하지 않고 저장 <br/>중복 저장 안됨 | HashSet, TreeSet|
|Map 계열|키와 값의 쌍으로 저장 <br/> 키는 중복 저장 안됨 | HashMap, Hashtable, <br/> TreeMap, Properties|

<br/>

<br/>

## 2. List 컬렉션
### 💡 List 특징
- List 컬렉션은 객체를 일렬로 늘어놓은 구조를 가지고 있다.
- 객체를 인덱스로 관리하기 때문에 객체를 저장하면 자동으로 인덱스가 부여된다.
- List 컬렉션은 객체 자체를 저장하는 것이 아니라 객체의 번지를 참조한다.

<br/>

### 💡 ArrayList 
- List 인터페이스의 구현 클래스로, 여기에 객체를 추가하면 객체가 인덱스로 관리된다.
    - 일반 배열과 인덱스로 객체를 관리한다는 점에서는 유사하다.
    - 다만 배열을 생성할 때 크기가 고정되고 사용중에 크기를 변경할 수 없지만,
    - ArrayList는 저장 용량(capacity)을 초과한 객체들이 들어오면 자동적으로 저장 용량이 늘어난다.

<br/>

다만, 객체를 삭제할 경우 바로 뒤 인덱스부터 모두 앞으로 1씩 당겨져야 하는 과정이 필요하다.

<br/> 

> 따라서 빈번한 객체 삭제와 삽입이 일어나는 곳에서는 `ArrayList`를 사용하는 것보다 `LinkedList`를 사용하는 것이 바람직하다.

<br/>

### 💡 Vector
- 내부 구조는 `ArrayList`와 동일하지만, `Vector`는 동기화된(synchronized) 메서드로 구성되어 있다.
- 멀티 스레드가 동시에 이 메서드를 실행할 수 없다.
    - 이를 스레드가 안전(Thread Safe)하다라고 말한다.

<br/>

### 💡 LinkedList
- LinkedList는 ArrayList와 사용 방법은 똑같지만 내부 구조는 완전 다르다.
- LinkedList는 인접 참조를 링크해서 체인처럼 관리한다.
  - 이때, 단일 연결이 아닌 이중 연결임을 기억하자!
- 특정 인덱스가 제거되면, 인접한 앞뒤 연결만 변경되면 된다.
  - **삭제와 삽입이 빈번히 일어날 때 좋은 성능을 발휘한다.**

<br/>

### 💡 ArrayList와 LinkedList 비교

|구분|    순차적으로 추가/삭제 | 중간에 추가/삭제| 검색 |
|:-------:|:------:|:-------:|:----:|
|ArrayList| 빠르다 | 느리다 | 빠르다 |
|LinkedList| 느리다 | 빠르다 | 느리다 |


<br/>

<br/>

## 3. Set 컬렉션
### 💡 Set 특징
- List와 다르게 Set 컬렉션은 저장 순서가 유지되지 않는다.
- 또한 객체를 중복해서 저장할 수 없고, 하나의 `null`만 저장할 수 있다.

<br/>

### 💡 Set 이해
- Set 컬렉션은 수학의 **집합**에 비유될 수 있다.
  - 집합은 순서와 상관없고, 중복이 허용되지 않는다.
  - 또한 저장할 때의 순서와 나올 때의 순서가 다를 수 있기 때문이다.

<br/>

### 💡 Set의 차별점
- Set 컬렉션은 인덱스로 객체를 검색해서 가져오는 메소드가 없다.
    - 대신 전체 객체를 대상으로 한번씩 반복해서 가져오는 반복자(Iterator)를 제공한다.
    - 반복자는 `Iterator` 인터페이스를 구현한 객체이고, `iterator()` 메소드를 호출해 얻는다.

<br/>

<details>
<summary>Iterator 사용 예시 알아보기</summary>
<div markdown="1">

```java
Set<String> set = ...;
Iterator<String> iterator = set.iterator();

while(iterator.hasNext()) { //먼저 가져올 객체가 있는지 확인한다.
	String str = iterator.next(); //객체를 리턴
	if(str.equals("홍길동")) {
		iterator.remove(); 
        //remove는 Iterator의 메소드이지만, 실제 Set 컬렉션에서 객체가 제거된다.
	}
}
```

</div>
</details>

<br/>

### 💡 HashSet
- HashSet은 객체들을 순서없이 저장하고 동일한 객체는 중복 저장하지 않는다.
    - 여기서 HashSet이 판단하는 동일한 객체란 꼭 같은 인스턴스를 뜻하지는 않는다.

<br/>

### 💡 HashSet의 객체 판단 과정
1. 객체를 저장하기 - 전에 먼저 객체의 hashCode() 메소드를 호출해 해시코드를 얻어낸다.
2. 이미 저장되어 있는 객체들의 해시코드와 비교한다.
    - 동일한 해시코드가 있다면 equals() 메소드로 두 객체를 비교한다.
3. true가 나오면 동일한 객체로 판단하여 저장하지 않는다.

<br/>

<br/>

## 4. Map 컬렉션
### 💡 Map 특징
- **Map 컬렉션은 키와 값으로 구성된 Entry 객체를 저장하는 구조**를 가지고 있다.
    - 여기서 키와 값은 모두 객체이다.
    - 키는 중복 저장될 수 없지만 값은 중복 저장될 수 있다.
    - 만약 기존에 저장된 키와 동일한 키로 값을 저장하면 새로운 값으로 대치된다.

<br/>

### 💡 HashMap
- HashMap은 Map 인터페이스를 구현한 대표적인 Map 컬렉션이다.
- HashMap의 키로 사용할 객체는 `hashCode()`와 `equals()` 메소드를 재정의해서 동등 객체가 될 조건을 정한다.

<br/>

### 💡 Hashtable
- Hashtable은 HashMap과 동일한 내부 구조를 가지고 있다.
- 차이점은 Hashtable은 동기화된(`synchronized`) 메소드로 구성되어 있다.

<br/>

### 💡 Properties
- 애플리케이션이나 데이터베이스 등의 정보가 저장된 프로퍼티(*.properties) 파일을 읽을 때 주로 사용된다.
- 이는 Hashtable의 하위 클래스이기 때문에 Hashtable의 모든 특징을 그대로 가진다.
  - 차이점은 Hashtable은 키와 값을 다양한 타입으로 지정이 가능한데 비해, Properties는 키와 값을 `String` 타입으로 제한한 컬렉션이다. 

<br/>

<br/>

## 5. 검색 기능을 강화시킨 컬렉션
### 💡 개요
 - 컬렉션 프레임워크는 검색 기능을 강화시킨 `TreeSet`과 `TreeMap`을 제공한다.
    - 이 컬렉션들은 이진 트리를 이용해서 계층적 구조를 가지면서 객체를 저장한다.

<br/>

### 💡 TreeSet
- 이진 트리를 기반으로 한 Set 컬렉션이다.

<br/>

#### 💡 사용하는 이유
- Set 인터페이스 타입 변수에 대입해도 되지만 TreeSet 클래스 타입으로 대입한 이유는,
- 객체를 찾거나 범위 검색과 관련된 메소드를 사용하기 위함

<br/>

### 💡 TreeMap
- 이진 트리를 기반으로 한 Map 컬렉션이다.
- TreeSet과의 차이점은 키와 같이 저장된 `Map.Entry`를 저장한다는 점이다.

<br/>

### 💡 Comparable과 Comparator
- TreeSet의 객체와 TreeMap의 키는 저장과 동시에 자동 오름차순으로 정렬된다.
    - 이때 정렬를 위해 `java.lang.Comparable`을 구현한 객체를 요구한다.
    - 사용자 정의 클래스도 Comparable을 구현한다면 자동 정렬이 가능하다.
        - Comparable에는 `compareTo()` 메소드가 정의되어 있다. 

<br/>

#### 💡 TreeSet 객체와 TreeMap의 키가 Comparable을 구현하고 있지 않을 경우
- 저장하는 순간 `ClassCastException`이 발생한다.

<br/>

<br/>

## 6. LIFO와 FIFO 컬렉션
### 💡 개요
- 후입선출(LIFO : Last In First Out) : 나중에 넣은 객체가 먼저 빠져나가는 자료구조
- 선입선출(FIFO : First In First Out) : 먼저 넣은 객체가 먼저 빠져나가는 자료구조

<br/>

> 컬렉션 프레임워크에는 LIFO 자료구조를 제공하는 스택(Stack) 클래스와 FIFO 자료구조를 제공하는 큐(Queue) 인터페이스를 제공하고 있다.

<br/>

### 💡 Stack
- 스택 클래스는 LIFO 자료구조를 구현한 클래스이다.
  - 대표적으로 JVM의 스택 메모리가 있다.

```java
Stack<E> stack = new Stack<E>();
```

<br/>

### 💡 Queue
- 큐 인터페이스는 FIFO 방식으로 동작한다.
    - Queue 인터페이스를 구현한 대표적인 클래스는 LinkedList이다.
    - 대표적으로 스레드풀의 작업 큐가 있다.

```java
Queue<E> queue = new LinkedList<E>();
```

<br/>

<br/>

## 7. 동기화된 컬렉션
### 💡 개요
- 컬렉션 프레임워크의 대부분의 클래스들은 싱글 스레드 환경에서 사용할 수 있도록 설계되었다.
- 따라서 여러 스레드가 동시에 컬렉션에 접근한다면 의도하지 않게 요소가 변경될 수 있는 불안전한 상태가 된다.

<br/>

> 따라서 컬렉션 프레임워크는 비동기화된 메소드를 동기화된 메소드로 래핑하는 Collections의 synchronizedXXX()의 메소드를 제공하고 있다. 

<br/>

#### 💡 동기화된 컬렉션으로 리턴하기

|리턴 타입|  메소드(매개 변수) | 설명|
|:-------:|:------|:-------|
|List<T> | synchronizedList(List<T> list) | List를 동기화된 List로 리턴|
|Map<K, V> | synchronizedMap(Map<K, V> m) | Map을 동기화된 Map으로 리턴|
|Set<T> | synchronizedSet(Set<T> s) | Set을 동기화된 Set으로 리턴|



<br/>

<br/>

## 8. 병렬 처리를 위한 컬렉션
### 💡 개요
- 동기화된(synchronized) 컬렉션은 멀티 스레드 환경에서 하나의 스레드가 요소를 안전하게 처리하도록 도와주지만, 전체 요소를 빠르게 처리하지는 못한다. 
    - 이는 하나의 스레드가 요소를 처리할 때 전체 잠금이 발생하여 다른 스레드는 대기 상태가 되기 때문이다.

<br/>

> 따라서 자바는 멀티 스레드가 병렬적으로 처리할 수 있도록 특별한 컬렉션을 제공한다. 

<br/>

### 💡 `ConcurrentHashMap`
- 스레드에 안전하면서도 멀티 스레드가 요소를 병렬적으로 처리할 수 있게 도와준다.

<br/>

```java
Map<K, V> map = new ConcurrentHashMap<K, V>();
```
- ConcurrentHashMap은 부분 잠금을 사용한다.
    - 전체 잠금 : 1개를 처리할 동안 전체 10개의 요소를 다른 스레드가 처리하지 못하도록 하는 것
    - 부분 잠금 : 처리하는 요소가 포함된 부분만 잠금하고, 나머지 부분은 다른 스레드가 변경할 수 있는 것

<br/>

### 💡 `ConcurrentLinkedQueue`
- 락-프리(lock-free) 알고리즘을 구현한 컬렉션이다.

<br/>

```java
Queue<E> queue = new ConcurrentLinkedQueue<E>();
```
- 락-프리 알고리즘은 여러 개의 스레드가 동시에 접근할 경우, 잠금을 사용하지 않고도 최소한 하나의 스레드가 안전하게 요소를 저장하거나 얻도록 해준다.

<br/>

## 💡마무리
글을 간략하게 작성하다보니 생략된 코드들이 있으니, 자세히 보려면 [링크](https://github.com/2dongyeop/thisisjava/tree/main/src/newCode/thisisjavaExercise/chapter15)를 클릭해주세요.