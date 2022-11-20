# 스트림과 병렬처리

- 본 내용은 [이것이 자바다 - 신용권]을 참고하여 정리하였습니다.
- [소스 코드 레퍼지토리](https://github.com/2dongyeop/thisisjava)로 이동하기

<br/>

> 1절. 스트림 소개
>
> 2절. 스트림의 종류
> 
> 3절. 스트림 파이프라인
> 
> 4절. 필터링(distinct(), filter())
>
> 5절. 매핑(flatMapXXX(), mapXXX(), asXXXStream(), boxed())
>
> 6절. 정렬(sorted())
>
> 7절. 루핑(peek(), forEach())
>
> 8절. 매칭(allMatch(), anyMatch(), noneMatch())
>
> 9절. 기본 집계(sum(), count(), average(), max(), min())
>
> 10절. 커스텀 집계(reduce())
>
> 11절. 수집(collect())
>
> 12절. 병렬 처리

<br/>

## 🔥 1. 스트림 소개
- 스트림(stream)이란?
  - 컬렉션의 저장 요소를 하나씩 참조해서 람다식으로 처리할 수 있도록 해주는 반복자로,
  - 자바 8부터 추가되었다.

<br/>

### 반복자 스트림
- 자바 7 이전 : Iterator 반복자를 사용하여 List<String> 컬렉션에서 요소를 순차적으로 처리
    ```java
    List<String> list = Arrays.asList("이", "동", "엽");
    Iterator<String> iterator = list.iterator();
    while(iterator.hasNext()) {
        String name = iterator.next();
        System.out.println(name);
    }
    ```

<br/>

- 자바 8 이후 : 스트림을 이용
    ```Java
    List<String> list = Arrays.asList("이", "동", "엽");
    Stream<String> stream = list.stream();
    stream.forEach( name -> System.out.println(name) );
    ```

<br/>

> 이때, `forEach()` 메소드는 아래와 같이 `Consumer` 함수적 인터페이스 타입의 매개값을 가진다.
> ```Java
> void forEach(Consumer<T> action)
> ```


<br/>

<br/>

### 스트림의 특징
- Stream은 Iterator와 비슷한 역할을 하는 반복자이지만, 크게 3가지의 차이가 있다.
  - **람다식으로 요소 처리 코드를 제공하는 점**이 다르다.
  - **내부 반복자를 사용하므로 병렬 처리가 쉽다.**
  - **중간 처리와 최종 처리 작업을 수행하는 점**에서 차이가 크다.

<br/>

#### 1. 람다식으로 요소 처리 코드를 제공한다.
- Stream이 제공하는 대부분의 요소 처리 메소드는 함수적 인터페이스 매개 타입을 가짐
  - 따라서 람다식이나 메소드 참조를 이용해 요소 처리 내용을 매개값으로 전달 가능!

    ```java
    public static void main(String[] args) {
        List<Student> list = Arrays.asList(
            new Student("홍길동", 90),
            new Student("신용권", 92)
        );

        Stream<Student> stream = list.stream();
        stream.forEach(s -> {
            String name = s.getName();
            int score = s.getScore();
            System.out.println(name + "-" + score);
        });
    }
    ```

<br/>

#### 2. 내부 반복자를 사용하므로 병렬 처리가 쉽다.
- 외부 반복자(external iterator)란?
  - 개발자가 코드로 직접 컬렉션의 요소를 반복해서 가져오는 코드 패턴을 말한다.
  - index를 이용하는 for문, Iterator를 이용하는 while문 모두 외부 반복자를 이용한다.

<br/>

- 내부 반복자(internal iterator)란?
  - 컬렉션 내부에서 요소들을 반복시킨다.
  - 개발자는 요소당 처리해야 할 코드만 제공하는 코드 패턴이다.

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/iterator2.png" width = 500/>

- 내부 반복자를 이용하면 얻는 이점
  - 컬렉션 내부에서 어떻게 요소를 반복시킬 것인가는 컬렉션에 맡긴다.
    - 이는 개발자가 요소 처리 코드에만 집중할 수 있게 된다.
  - 요소들의 반복 순서를 변경하거나, 멀티 코어 CPU를 최대한 활용하기 위해 요소들을 분배한다.
    - 이는 병렬 작업에 도움이 된다.
    
<br/>

- 병렬(parallel) 처리란?
  - 한 가지 작업을 서브 작업으로 나누고, 서브 작업들을 분리된 스레드에서 병렬 처리를 하는 것

<br/> 

> 이 내용은 따로 정리를 한 [링크](https://github.com/2dongyeop/TIL/blob/main/Java/iterator.md)를 첨부합니다.

<br/>

#### 3. 중간 처리와 최종 처리 작업을 수행한다.
- 스트림은 컬렉션의 요소에 대해 중간 처리와 최종 처리를 수행할 수 있다.
  - 중간 처리 → 매핑, 필터링, 정렬
  - 최종 처리 → 반복, 카운팅, 평균, 총합 등의 집계 처리


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/중간처리-최종처리.png" width = 500/>

<br/>

<details>
<summary>예시 소스 코드</summary>
<div markdown="1">

```java
public class MapAndReduceExample {
    public static void main(String[] args) {
        List<Student> studentList = Arrays.asList(
            new Student("홍길동", 10),
            new Student("신용권", 20),
            new Student("유미선", 30)
        );

        double avg = studentList.stream()
          .mapToInt(Student::getScore)
          .average()
          .getAsDouble();

        System.out.println("평균 점수: " + avg);
    }
}
```

</div>
</details>

<br/>

<br/>

## 🔥 2. 스트림의 종류
자바 8부터 새로 추가된 java.util.stream 패키지에는 스트림 API들이 포진하고 있다.

- BaseStream 인터페이스
  - 모든 스트림에서 사용할 수 있는 공통 메소드들이 정의되어 있을 뿐 직접적으로 사용하지 않는다.
  - Stream 
    - 객체 요소를 처리하는 스트림
  - IntStream, LongStream, DoubleStream 
    - 각각 해당 기본 타입 요소를 처리하는 스트림

<br/>

> 스트림 인터페이스의 구현 객체는 컬랙션과 배열 외에도 다양한 소스로부터 얻을 수 있다.

<br/>

### 컬렉션으로부터 스트림 얻기
```java
public class FromCollectionExample {
    public static void main(String[] args) {
        List<Student> studentList = Arrays.asList(
            new Student("홍길동", 10),
            new Student("신용권", 20),
            new Student("유미선", 30)
        );

        Stream<Student> stream = studentList.stream();
        stream.forEach(s -> System.out.println(s.getName()));
    }
}
```

<br/>

### 배열로부터 스트림 얻기
```java
public class FromArrayExample {
    public static void main(String[] args) {
        String[] strArray = {"홍길동", "신용권", "김미나"};
        Stream<String> strStream = Arrays.stream(strArray);
        strStream.forEach(a -> System.out.print(a + ","));

        System.out.println();

        int[] intArray = {1, 2, 3, 4, 5};
        IntStream intStream = Arrays.stream(intArray);
        intStream.forEach(a -> System.out.print(a + ","));
    }
}
```

<br/>

### 숫자 범위로부터 스트림 얻기
```java
public class FromIntRangeExample {
    public static int sum;

    public static void main(String[] args) {
        IntStream stream = IntStream.rangeClosed(1, 100); /* 1부터 100까지 */
        /* IntStream stream = IntStream.range(1, 100); 1부터 99까지 */
        stream.forEach(a -> sum += a);
        System.out.println("총합: " + sum);
    }
}
```

<br/>

<br/>

