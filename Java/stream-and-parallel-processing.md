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

<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/iterator2.png" width = 700/>

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


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/중간처리-최종처리.png" width = 700/>

#### 예제 코드
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

## 🔥 3. 스트림 파이프라인
- 리덕션(Reductuon)이란?
  - 대량의 데이터를 가공해 축소하는 것

<br/>

- 컬렉션의 요소를 리덕션의 결과물(합계,평균 등)로 바로 집계할 수 없을 경우엔?
  - 집계하기 좋도록 필터링, 매핑, 정렬, 그룹핑 등의 중간 처리가 필요하다.

<br/>

### 중간 처리와 최종 처리
- 스트림은 중간 처리와 최종 처리를 파이프라인(pipelines)으로 해결한다.

<br/>

- 파이프라인이란?
  - 파이프라인은 여러 개의 스트림이 연결되어 있는 구조이다.
  - 내부에 최종 처리를 제외하곤 모두 중간 처리 스트림이다.

<br/>

💡 중간 스트림이 생성될 때 요소들이 바로 중간 처리되는 것이 아님을 기억하자!
  - 최종 처리가 시작되기 전까지 중간 처리는 지연(lazy)되고,
  - 최종 처리가 시작되면 비로소 컬렉션의 요소가 하나씩 중간 스트림에서 처리되어 최종까지 온다.

<br/>

#### 예제 코드
- 파이프라인을 자바 코드로 구현
```java
Stream<Member> maleFemaleStream = list.stream();
Stream<Member> maleStream = maleFeamleStream.filter(m -> m.getSex() == Member.MALE);
IntStream ageStream = maleStream.mapToInt(Member :: getAge);
OptionalDouble optionalDouble = ageStream.average();
double ageAvg = optionalDouble.getAsDouble();
```

<br/>

- 로컬 변수를 생략하고 연결하여 만든 파이프라인
```java
/*
회원 목록에서 성별이 남성인 회원의 평균 나이 구하기
*/

double ageAvg = list.stream()     //오리지날 스트림
  .filter(m -> m.getSex() == Member.MALE)  //중간 처리 스트림
  .mapToInt(Member :: getAge)     //중간 처리 스트림
  .average()                      //최종 처리 스트림
  .getAsDouble();
```

<br/>

### 중간 처리 메소드와 최종 처리 메소드
- 중간 처리와 최종 처리를 쉽게 구분하는 방법은 리턴타입을 보면 된다.
  - 중간 처리 메소드 → 리턴 타입이 스트림이다.
  - 최종 처리 메소드 → 리턴 타입이 기본 타입 혹은 `OptionalXXX` 이다.


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/pipeline-method.png" width = 700/>

<br/>

<br/>

## 🔥 4. 필터링
- 필터링은 중간 처리 기능으로 요소를 걸러내는 역할을 한다.
- 아래 두 필터링 메소드는 모든 스트림이 가지고 있는 공통 메소드이다.
  - `distinct()`
  - `filter()`

<br/>

- `distinct()` : 중복을 제거
  - Stream의 경우 `Object.equals(Object)`가 true이면 동일한 객체로 판단하여 중복을 제거한다.

<br/>

- `filter()` : 조건을 필터링
  - 매개값으로 주어진 Predicate가 true를 리턴하는 요소만 필터링한다.


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/filtering-method.png" width = 600/>


#### 예제 코드
```java
public class FilteringExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("홍길동", "신용권", "감자바", "신용권", "신민철");

        names.stream()
            .distinct()        //중복 제거
            .forEach(n->System.out.println(n));  //홍길동 신용권 김자바 신민철
        System.out.println();  

        names.stream()
            .filter(n->n.startsWith("신"))  //필터링
            .forEach(n->System.out.println(n));  //신용권 신용권 신민철
        System.out.println(); 

        names.stream()
            .distinct()         //중복 제거 후 필터링
            .filter(n->n.startsWith("신"))
            .forEach(n->System.out.println(n));  //신용권 신민철
    }
} 
```

<br/>

<br/>

## 🔥 5. 매핑
- 중간 처리 기능 중 하나로,, 스트림의 요소를 다른 요소로 대체하는 작업이다.
- 스트림에서 제공하는 매핑 메소드는 아래와 같다.
  - `flatXXX()`
  - `mapXXX()`
  - `asDoubleStream()`
  - `boxed()`

<br/>

### `flatXXX()` 메소드
- 이 메소드는 요소를 대체하는 복수 개의 요소들로 구성된 새로운 스트림을 리턴한다.


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/flat-method.png" width = 600/>


### 예제 코드
```java
/*
입력된 데이터(요소)들이 List<String>에 저장되어 있다고 가정하고, 
요소별로 단어를 뽑아 단어 스트림으로 재생성한다.
만약 데이터가 숫자라면 숫자를 뽑아 숫자 스트림으로 재생성한다.
*/

public class FlatMapExample {
    public static void main(String[] args) {
        List<String> inputList1 = Arrays.asList("java8 lambda", "stream mapping");

        inputList1.stream()
            .flatMap(data->Arrays.stream(data.split(" ")))
            .forEach(word->System.out.println(word));
        System.out.println();

        List<String> inputList2 = Arrays.asList("10, 20, 30", "40, 50, 60");

        inputList2.stream()
            .flatMapToInt(data->{
                String[] strArray = data.split(",");
                int[] intArr = new int[strArray.length];
                for (int i = 0; i < strArray.length; i++) {
                    intArr[i] = Integer.parseInt(strArray[i].trim());
                }
                return Arrays.stream(intArr);
            })
            .forEach(number->System.out.println(number));
    }
}
```

<br/>

### `mapXXX()` 메소드
- 이 메소드는 요소를 대체하는 요소로 구성된 새로운 스트림을 리턴한다.


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/map-method.png" width = 600/>

#### 예제 코드
```java
/*
학생 List에서 학생의 점수를 요소로 하는 새로운 스트림을 생성하고, 
점수를 순차적으로 출력한다.
*/

public class MapExample {
    public static void main(String[] args) {
        List<Student> studentList = Arrays.asList(
            new Student("홍길동", 10),
            new Student("신용권", 20),
            new Student("유미선", 30)
        );

        studentList.stream()
            .mapToInt(Student::getScore)
            .forEach(score->System.out.println(score));
    }
}
```

<br/>

### `boxed()`, `asDoubleStream()`, `asLongStream()` 메소드
- `boxed()`
  - int요소, long요소, double요소를 Integer, Long, Double요소로 박싱해서 Stream을 생성
- `asDoubleStream()`
  - IntStream()의 int요소 또는 LongStream의 long요소를 double요소로 타입 변환해서 DoubleStream을 생성
- `asLongStream()`
  - IntStream()의 int요소를 long요소로 타입 변환해서 LongStream을 생성

<br/>

#### 예제 코드
```java
/*
int[] 배열로 IntStream을 얻고, double로 타입 변환해 DoubleStream을 생성한다.
또한 int 요소를 Integer 객체로 박싱해 Stream<Integer>를 생성한다.
*/
public class AsDoubleStreamAndBoxedExample {
    public static void main(String[] args) {
        int[] intArray = {1, 2, 3, 4, 5};

        IntStream intStream = Arrays.stream(intArray);
        intStream.asDoubleStream().forEach(d -> System.out.println(d));

        System.out.println();

        intStream = Arrays.stream(intArray);
        intStream.boxed().forEach(obj -> System.out.println(obj.intValue()));
    }
}
```

<br/>

<br/>

## 🔥 6. 정렬
- 스트림은 요소가 최종 처리되기 전에 중간 단계에서 **요소를 정렬**해서 최종 처리 순서를 변경할 수 있다.

<br/>


|리턴 타입|메소드(매개변수)|설명|
|:--:|:---:|:---:|
|Stream<T>|sorted()|객체를 `Comparable` 구현 방법에 따라 정렬|
|Stream<T>|sorted(Comparator<T>)|객체를 주어진 `Comparator`에 따라 정렬|
|DoubleStream|sorted()|double 요소를 오름차순으로 정렬|
|IntStream|sorted()|int 요소를 오름차순으로 정렬|
|LongStream|sorted()|long 요소를 오름차순으로 정렬|

<br/>

- 객체 요소일 경우 : `Comparable`을 구현한 요소에서만 `sorted()` 메소드를 호출할 수 있다.
  - 구현하지 않고 메소드를 호출하면 `ClassCastException`이 발생한다.
    ```java
    /* 점수를 기준으로 Student 요소를 오름차순으로 정렬 */
    public class Student2 implements Comparable<Student> {
        private String name;
        private int score;

        public Student2(String name, int score) {
            this.name = name;
            this.score = score;
        }

        public String getName() {
            return name;
        }
        public int getScore() {
            return score;
        }

        @Override
        public int compareTo(Student2 o) {
            return Integer.compare(score, o.score);
        }
    }
    ```

<br/>

- 객체 요소가 `Comparable`을 구현한 상태에서 기본 비교(`Comparable`)로 정렬하는 3가지 방법
  ```java
  sorted();
  sorted( (a, b) -> a.compareTo(b) );
  sorted( Comparator.naturalOrder() );
  ```

<br/>

- `Comparable`을 구현했지만, 기본 비교(`Comparable`)와 반대로 역순으로 정렬하는 2가지 방법
  ```java
  sorted( (a,b) -> b.compareTo(a) );
  sorted( Comparator.reverseOrder() );
  ```

<br/>

- 객체 요소가 `Comparable`을 구현하지 않았다면, `Comparator`를 매개값으로 갖는 `sorted()`를 호출하자.
  - `Comparator`는 함수적 인터페이스이므로 다음과 같이 람다식으로 매개값을 작성할 수 있다.
  - 중괄호 안에는 a와 b를 비교해서 a가 작으면 음수, 같으면 0, a가 크면 양수를 리턴하는 코드를 작성하면 된다.
    ```java
    sorted((a, b) -> { ... })
    ```

<br/>

#### 예제 코드
```java
public class SortingExample {
    public static void main(String[] args) {
        IntStream intStream = Arrays.stream(new int[] {5, 3, 2, 1, 4});

        /* 숫자 요소일 경우 오름차순으로 정렬 후 출력 */

        intStream
            .sorted()
            .forEach(n->System.out.println(n + ","));
        System.out.println();  //1,2,3,4,5,

        List<Student2> studentList = Arrays.asList(
            new Student2("홍길동", 30),
            new Student2("신용권", 10),
            new Student2("유미선", 20)
        );

        /* 객체 요소일 경우 기본 비교(Comparable) 방법을 이용해 오름차순으로 정렬 후 출력 */
        studentList.stream()
            .sorted()
            .forEach(s->System.out.println(s.getScore() + ","));
        System.out.println();   //10,20,30,

        /* Comparator를 제공해 점수를 기준으로 내림차순으로 정렬 후 출력 */
        studentList.stream()
            .sorted(Comparator.reverseOrder())
            .forEach(s->System.out.println(s.getScore() + ","));
        System.out.println();   //30,20,10,
    }
}
```

<br/>

<br/>

## 🔥 7. 루핑
- 루핑(looping)은 요소 전체를 반복하는 것을 말한다. 
  - 루핑하는 메소드에는 `peek()`, `forEach()`가 있다.

<br/>

- 이 두 메소드는 루핑한다는 기능은 같지만 동작 방식이 다르다.
  - `peek()` : 중간 처리 메소드
  - `forEach()` : 최종 처리 메소드

<br/>

- `peek()`
  - 중간 처리 단계에서 전체 요소를 루핑하면서 추가적인 작업을 하기 위해 사용한다.
  - 최종 처리 메소드가 실행되지 않으면 지연되기 때문에 **반드시 최종 처리 메소드가 호출되어야 작동**한다. 
    - 이는 위에서 말했듯 중간 처리 메소드이기 때문이다.

  <br/>

  - 동작하지 않는 예시  
    ```java
    intStream
      .filter(a -> a % 2 == 0)
      .peek(System.out::println);
    ```

  <br/>

  - 정상 동작하는 예시
    ```java
    intStream
      .filter(a -> a % 2 == 0)
      .peek(System.out::println)
      .sum()
    ```

<br/>

- `forEach()`
  - 최종 처리 메소드이기 때문에 파이프라인 마지막에 루핑하면서 요소를 하나씩 처리한다.
  - 요소를 소비하는 최종 처리 메소드이므로 **다른 최종 메소드를 호출하면 안된다.**

<br/>

#### 예제 코드
```Java
public class LoopingExample {
    public static void main(String[] args) {
        int[] intArr = {1, 2, 3, 4, 5};

        System.out.println("[peek()를 마지막으로 호출한 경우]");
        Arrays.stream(intArr)
            .filter(a->a%2==0)
            .peek(n->System.out.println(n)); // 동작하지 않음

        System.out.println("[최종 처리 메소드를 마지막에 호출한 경우]");
        int total = Arrays.stream(intArr)
            .filter(a->a%2==0)
            .peek(n->System.out.println(n))  // 2 4
            .sum();
        System.out.println("총합: " + total); // 총합: 6

        System.out.println("[forEach()를 마지막에 호출한 경우]");
        Arrays.stream(intArr)
            .filter(a->a%2==0)
            .forEach(n->System.out.println(n)); // 2 4
    }
}
```
<br/>

<br/>

## 🔥 8. 매칭
- 스트림 클래스는 최종 처리 단계에서 요소들이 **특정 조건에 만족하는지 조사**할 수 있도록 메소드를 제공한다.
  - `allMatch()` : **모든 요소**들이 매개값으로 주어진 `Predicate`의 **조건을 만족하는지** 조사
  - `anyMatch()` : **최소한 한 개**의 요소가 매개값으로 주어진 `Predicate`의 **조건을 만족하는지** 조사
  - `nonMatch()` : **모든 요소**들이 매개값으로 주어진 `Predicate`의 **조건을 만족하는지 않는지** 조사

<br/>

#### 예제 코드
```java
/* int[] 배열로부터 스트림을 생성하고, 요소들이 각각 특정 조건에 만족하는지를 조사한다. */
public class MatchExample {
    public static void main(String[] args) {
        int[] intArr = {2, 4, 6};

        boolean result = Arrays.stream(intArr).allMatch(a -> a % 2 == 0);
        System.out.println("모두 2의 배수인가? " + result);

        result = Arrays.stream(intArr).anyMatch(a -> a % 3 == 0);
        System.out.println("하나라도 3의 배수가 있는가? " + result);

        result = Arrays.stream(intArr).noneMatch(a -> a % 3 == 0);
        System.out.println("3의 배수가 없는가? " + result);
    }
}
```

<br/>

<br/>

## 🔥 9. 기본 집계
- 집계(`Aggregate`)란?
  - 최종 처리 기능으로 요소들을 처리해서 하나의 값으로 산출하는 것을 말한다.
    - ex) 카운팅, 합계, 평균값, 최대값, 최소값 등
  - 집계는 대량의 데이터를 가공해서 축소하는 리덕션(`Reduction`)이라고 볼 수 있다.

<br/>

### 스트림이 제공하는 기본 집계
- 자바 8에서는 `java.util` 패키지에 `OptionalXXX`이 추가되었다.
- 이들은 값을 저장하는 값 기반 클래스(`value-based class`)들로, 다음 절에서 나올 예정이다.

|리턴타입	|메소드(매개변수)	|설명|
|:---|:---|:---:|
|long	|count()	|요소 개수|
|OptionalXXX	|findFirst()	|첫번째 요소|
|Optional<T>	|max(Compatator<T>)	|최대 요소|
|OptionalXXX	|max()	|최대 요소|
|Optional<T>	|min(Comparator<T>)	|최소 요소|
|OptionalXXX	|min()	|최소 요소|
|OptionalDouble|	average()	|요소 평균|
|int, long, double|	sum()|	요소 총합|

<br/>

#### 예제 코드
```java
public class AggregateExample {
    public static void main(String[] args) {
        long count = Arrays.stream(new int[] {1, 2, 3, 4, 5})
            .filter(n -> n % 2 == 0)
            .count();
        System.out.println("2의 배수 개수: " + count);
        
        long sum = Arrays.stream(new int[] {1, 2, 3, 4, 5})
            .filter(n -> n % 2 == 0)
            .sum();
        System.out.println("2의 배수의 합: " + sum);
        
        int max = Arrays.stream(new int[] {1, 2, 3, 4, 5})
            .filter(n -> n % 2 == 0)
            .max()
            .getAsInt();
        System.out.println("최댓값: " + max);

        int min = Arrays.stream(new int[] {1, 2, 3, 4, 5})
            .filter(n -> n % 2 == 0)
            .min()
            .getAsInt();
        System.out.println("최솟값: " + min);

        int first = Arrays.stream(new int[] {1, 2, 3, 4, 5})
            .filter(n -> n % 3 == 0)
            .findFirst()
            .getAsInt();
        System.out.println("첫번째 3의 배수: " + first);
    }
}
```

<br/>

### `Optional` 클래스
- 이 클래스들은 저장하는 값의 타입만 다를 뿐 제공하는 기능은 거의 동일하다.
  - 단, 단순히 집계 값만 저장하는 것이 아니다!
    - 집계 값이 존재하지 않을 경우 디폴트 값을 설정할 수도 있고,
    - 집계 값을 처리하는 `Consumer`도 등록할 수 있다.

|리턴 타입 | 메소드(매개변수) | 설명|
|:---:|:---:|:---:|
|boolean|isPresent()|값이 저장되어 있는지 여부|
|T|orElse(T)|값이 저장되어 있지 않을 경우 디폴트 값 지정|
|double|orElse(double)|값이 저장되어 있지 않을 경우 디폴트 값 지정|
|int|orElse(int)|값이 저장되어 있지 않을 경우 디폴트 값 지정|
|long|orElse(long)|값이 저장되어 있지 않을 경우 디폴트 값 지정|
|void|ifPresent(Consumer)|값이 저장되어 있지 않을 경우 Consumer에서 처리|
|void|ifPresent(DoubleConsumer)|값이 저장되어 있지 않을 경우 Consumer에서 처리|
|void|ifPresent(IntConsumer)|값이 저장되어 있지 않을 경우 Consumer에서 처리|
|void|ifPresent(LongConsumer)|값이 저장되어 있지 않을 경우 Consumer에서 처리|

<br/>

- 컬렉션의 요소는 **동적으로 추가**되는 경우가 많다.
  - 만약 컬렉션의 요소가 추가되지 않아 저장된 요소가 없을 경우는? → `NoSuchElementException` 발생
    ```java
    List<Integer> list = new ArrayList<>();
    double avg = list.stream()
      .mapToInt(Integer :: intValue)
      .average()
      .getAsDouble();
    System.out.println("평균: " + avg);
    ```

<br/>

- `NoSuchElementException`을 피하는 3가지 방법
  - `Optional` 객체를 얻어 `isPresent()` 메소드로 평균값 여부를 확인
    ```java
    OptionalDouble optional = list.stream()
      .mapToInt(Integer :: intValue)
      .average();
    if(optional.isPresent()) {
      System.out.println("평균: " + optional.getAsDouble());
    } else {
      System.out.println("평균: 0.0");
    }
    ```
  <br/>

  - `orElse()` 메소드로 디폴트 값을 정해놓기
    ```java
    double avg = list.stream()
      .mapToInt(Integer :: intValue)
      .average()
      .orElse(0.0);
    System.out.println("평균: " + avg);
    ```

  <br/>

  - `ifPresent()` 메소드로 평균값이 있을 경우에만 값을 이용하는 람다식 실행
    ```java
    list.stream()
      .mapToInt(Integet :: intValue)
      .average()
      .ifPresent(a -> System.out.println("평균: " + a));
    ```

<br/>

<br/>

## 🔥 10. 커스텀 집계
- 스트림은 프로그램화해서 다양한 집계 결과물을 만들 수 있도록 `reduce()` 메소드를 제공한다.

<br/>

|인터페이스|	리턴타입|	메소드(매개변수)|
|:---|:---|:---|
|Stream|	Optional<T>|	reduce(BinaryOperator<T> accumulator)|
|Stream	|T	|reduce(T identity, BinaryOperator<T> accumulator)|
|IntStream	|OptionalInt	|reduce(IntBinaryOperator op)|
|IntStream	|int	|reduce(int identity, IntBinaryOperator op)|
|LongStream	|OptionalLong|	reduce(LongBinaryOperator op)|
|LongStream	|long	|reduce(long identity, LongBinaryOperator op)|
|DoubleStream	|OptionalDouble|	reduce(DoubleBinaryOperator op)|
|DoubleStream	|double	|reduce(double identity, DoubleBinaryOperator op)|

<br/>

- 위 표에서 인터페이스 타입별로 두 번째 메소드를 통해 다음을 알 수 있다.
  - 스트림에 요소가 없어 `NoSuchElementException`이 발생할 경우,
  - 첫 번째 매개변수인 `identity`가 디폴트 값으로 리턴된다.

<br/>

- 코드를 통한 디폴트 값 차이 리뷰
```java
/* identity가 매개변수에 전달되지 않아, 요소가 없을 경우 예외 발생 */
int sum = studentList.stream()
  .map(Student::getScore)
  .reduce((a, b) -> a + b)
  .get();

/* identity = 0으로 전달되어, 요소가 없을 경우 디폴트 값인 0을 리턴 */
int sum = studentList.stream()
  .map(Student::getScore)
  .reduce(0, (a, b) -> a + b)
  .get();
```

<br/>

<br/>

## 🔥 11절. 수집
- 스트림은 요소들을 필터링/매핑한 후 요소들을 수집하는 최종 처리 메소드인 `collect()`를 제공한다.
- 이 메소드를 이용하여 **필요한 요소들만 컬렉션으로 담을 수 있고**, 요소들을 그룹핑한 후 집계(리덕션)할 수 있다.

<br/>

### 필터링한 요소 수집
|리턴 타입|메소드(매개 변수)|인터페이스|
|:---:|:---:|:---:|
|R|collect(Collector(T, A, R) collector|Stream|

- 매개값인 Collector(수집기)는 어떤 요소를 어떤 컬렉션에 수집할 것인지를 결정한다.
  - T는 요소, A는 누적기(accumulator), R은 요소가 저장될 컬렉션이다.
  - 풀어쓰면, T 요소를 A 누적기가 R에 저장한다는 의미이다.

<br/>

`Collector`의 구현 객체는 아래와 같이 `Collectors` 클래스의 다양한 정적 메소드를 이용해서 얻을 수 있다.

<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/Collectors-method.png" width = 700/>

- 리턴값이 Collector를 보면 A(누적기)가 ?로 되어 있다.
  - 이는 Collector가 R (컬렉션)에 T(요소)를 저장하는 방법을 알고 있어 A(누적기)가 필요 없기 때문이다. 

<br/>

> 여기서 **Map은 스레드에 안전하지 않고**, **ConcurrentMap은 스레드에 안전**하기 때문에, 
>
> 멀티 스레드 환경에서 사용하려면 ConcurrentMap을 얻는 것이 좋다.

<br/>

#### 예제 코드
- 스트림을 자바 코드로 구현
  ```java
  /* 전체 학생 List에서 남학생들만 별도의 List로 수집 */
  Stream<Student> totalStream = totalList.stream();
  Stream<Student> maleStream = totalStream.filter(s->s.getSex() ==Student.Sex.MALE);
  Collector<Student, ?, List<Student>> collector = Collectors.toList();

  List<Student> maleList = maleStream.collect(collector);
  ```

<br/>

- 위 코드를 변수를 생략하고 간단하게 작성
  ```java
  List<Student> maleList = totalList.stream()
    .filter(s->s.getSex() == Student.Sex.MALE)
    .collect(Collectors.toList());
  ```

<br/>

<br/>

### 사용자 정의 컨테이너에 수집하기
- 이번에는 `List`, `Set`, `Map`과 같은 컬렉션이 아니라 사용자 정의 컨테이너 객체에 수집하는 방법이다.

<br/>

- 스트림은 요소들을 필터링, 또는 매핑해서 사용자 정의 컨테이너 객체에 수집할 수 있는 `collect()`를 제공한다.

|인터페이스|	리턴타입	|메소드(매개변수)|
|:---:|:---:|:---|
|Stream	|R|	collect(Supplier<R>, BiConsumer<R, ?, super T>, BiConsumer<R, R>)|
|IntStream	|R|	collect(Supplier<R>, ObjIntConsumer<R>, BiConsumer<R, R>)|
|LongStream	|R|	collect(Supplier<R>, ObjLongConsumer<R>, BiConsumer<R, R>)|
|DoubleStream	|R|	collect(Supplier<R>, ObjDoubleConsumer<R>, BiConsumer<R, R>)|

- 위 표를 보면 아래와 같이 정리할 수 있다.
  - 첫 번째 매개변수 `Supplier`는 요소들이 수집될 컨테이너 객체(R)를 생성하는 역할을 한다.
    - 순차 처리(싱글 스레드) 스트림 → 단 한 번 `Supplier`가 실행되고 하나의 컨테이너 객체 생성
    - 병렬 처리(멀티 스레드) 스트림 → 여러 번 `Supllier`가 실행되고 스레드 별 여러 컨테이너 객체 생성
  - 두 번째 매개변수 `XXXConsumer`는 컨테이너 객체(R)에 요소(T)를 수집하는 역할을 한다.
  - 세 번째 매개변수 `BiConsumer`는 컨테이너 객체(R)를 결합하는 역할을 한다.
    - 순차 처리 스트림에서는 호출되지 않는다.
    - 병렬 처리 스트림에서는 호출되어 스레드별로 생성된 컨테이너 객체를 결합해서 최종 컨테이너 객체를 완성한다.
  
  <br/>

  - 리턴 타입 R은 요소들이 최종 수집된 컨테이너 객체이다.
    - 순차 처리 스트림에서는 리턴 객체가 첫 번째 `Supplier`가 생성하지만,
    - 병렬 처리 스트림에서는 최종 결합된 컨테이너 객체가 된다.

<br/>

#### 예제 코드
- 스트림을 예제 코드로 구현
  ```java
  Stream<Student> totalStream = totalList.stream();
  Stream<Student> maleStream = totalStream.filter(s-> s.getSex() ==Student.Sex.MALE);

  Supplier<MaleStudent> supplier = () -> new MaleStudent();
  BiConsumer<MaleStudent, Student> accumulator = (ms, s)-> ms.accumulate(s);
  BiConsumer<MaleStudent, MaleStudent> combiner = (ms1, ms2) -> ms1.combine(ms2);

  MaleStudent maleStudent = maleStream.collect(supplier, accumulator, combiner);
  ```

<br/>

### 요소를 그룹핑해서 수집
- `collect()` 메소드는 수집하는 기능외에 컬렉션의 요소들을 **그룹핑해서 Map 객체를 생성하는 기능도 제공**한다. 
  - `collect()`를 호출할 때 Collectors의 정적 메소드가 리턴하는 Collectors를 매개값으로 대입하면 된다.
  <img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/grouping-collector.png" width = 700/>

- `groupingBy()` → 스레드에 안전하지 않은 Map을 생성
- `groupingByConcurrent()` → 스레드에 안전한 ConcurrentMap을 생성

<br/>

#### 예제 코드
```java
/* 학생의 성별로 그룹핑하여 List를 생성한 후, 성별을 키로, 학생 List를 값으로 갖는다. */

Map<Student.Sex, List<Student>> mapBySex = totalList.stream()
	.collect(Collectors.groupingBy(Student :: getSex));
```

<br/>

### 그룹핑 후 매핑 및 집계
- `Collectors.groupingBy()` 메소드는 그룹핑 후, 매핑이나 집계를 할 수 있도록 두 번째 매개값으로 Collector를 가질 수 있다.

  <img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/grouping-by.png" width = 700/>


<br/>

#### 예제 코드
```java
/* 학생들을 성별로 그룹핑하고, 같은 그룹의 학생들의 평균 점수를 구한 뒤 성별을 키로, 평균 점수를 값으로 갖는 Map을 생성 */
Map<Student.Sex, Double> mapBySex = totalList.stream()
  .collect(
    Collectors.groupingBy(
      Student :: getSex,
      Collectors.averagingDouble(Student :: getScore)
    )
); 
```

<br/>

<br/>

## 🔥 12. 병렬 처리
### 병렬 처리(Parallel Operation)
- 멀티 코어 CPU 환경에서 하나의 작업을 분할 → 각각의 코어가 병렬적으로 처리하는 것

<br/>

#### 병렬 처리(Parallel Operation)의 목적
- 작업 처리 시간을 줄이기 위함

<br/>

#### 동시성(Concurrency)과 병렬성(Parallelism)
- 멀티 스레드는 동시성과 병렬성으로 실행되기 때문에 이 용어들에 대해 정확히 이해하는 것이 좋다.
- 이 둘은 멀티 스레드의 동작 방식이라는 점에서는 동일하지만, **서로 다른 목적**을 가지고 있다.


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/동시성-병렬성.png" width = 700/>

- **동시성** : 멀티 작업을 위해 **멀티 스레드가 번갈아가며 실행**하는 성질
- **병렬성** : 멀티 작업을 위해 **멀티 코어를 이용해서 동시에 실행**하는 성질

<br/>

#### 병렬성 구분
- 데이터 병렬성
  - **전체 데이터를 쪼개어 서브 데이터로 만듬 → 이 들을 병렬 처리로 작업을 빨리 끝내는 것**
  - 자바 8에서 병렬 스트림은 데이터 병렬성을 구현한 것

<br/>

- 작업 병렬성
  - **서로 다른 작업을 병렬 처리하는 것**
  - 대표적인 예는 웹 서버다.
    - 각각의 브라우저에서 요청한 내용을 개별 스레드에서 병렬로 처리한다.

<br/>

### 포크조인(ForkJoin) 프레임워크
- 병렬 스트림은 요소들을 병렬 처리하기 위해 포크조인 프레임워크를 사용한다.


<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/fork-join.png" width = 700/>

<br/>

동작 과정
1. 병렬 스트림을 이용 → 런타임 시 프레임워크가 동작 
2. **포크 단계에서 전체 데이터를 서브 데이터로 분리**
3. 서브 데이터를 멀티 코어에서 병렬로 처리
4. **조인 단계에서 서브 결과를 결합해 최종 결과를 만들어냄**

<br/>

> 단, 실제로 포크 단계에서 차례대로 요소를 N등분 하지는 않는다. 
>
> → 내부적으로 서브 요소를 나누는 **알고리즘**이 있다.
>
> <br/>
>
> 포크조인 프레임워크에는 포크와 조인 기능 이외에 **스레드풀인 ForkJoinPool을 제공**한다.
>
> → 각각의 코어에서 서브 요소를 처리하는 것은 개별 스레드가 해야 하므로 스레드 관리가 필요하다.

<br/>

### 병렬 스트림 생성
- 병렬 처리를 위해 코드에서 직접 포크조인 프레임워크를 사용할 순 있지만,
- 병렬 스트림을 이용하면? → 백그라운드에서 프레임워크가 사용됨 → 매우 쉬운 병렬 처리 가능하다!

<br/>

|인터페이스|	리턴타입	|메소드(매개변수)|
|---|----|----|
|java.util.Collection	|Stream	|parallelStream()|
|java.util.Stream|	Stream	|parallel()|
|java.util.IntStream|	IntStream|	parallel()|
|java.util.LongStream	|LongStream	|parallel()|
|java.util.DoubleStream	|DoubleStream	|parallel()|

<br/>

#### 예제 코드
```java
MaleStudent maleStudent = totalList.parallelStream()
	.filter(s->s.getSex() == Student.Sex.MALE)
	.collect(MaleStudent :: new, MaleStudent :: accumulate, MaleStudent :: combine);
```

<br/>

### 병렬 처리 성능
- 병렬 처리는 항상 빠르다?
  - 스트림 병렬 처리가 스트림 순차 처리보다 항상 실행 성능이 좋다고 판단해서는 안된다.

<br/>

- **병렬 처리에 영향을 미치는 3가지 요인**
  - 요소의 수와 요소당 처리 시간
    - 컬렉션에 요소의 수가 적고 요소당 처리 시간이 짧을 경우 → 순차 처리가 오히려 병렬 처리보다 빠를 수 있다. 
    - 병렬 처리는 스레드풀 생성, 스레드 생성이라는 추가적인 비용이 발생하기 때문이다.
  - 스트림 소스의 종류
    - ArrayList, 배열은 랜덤 액세스를 지원(인덱스로 접근)하기 때문에 포크 단계에서 쉽게 요소를 분리할 수 있어 병렬 처리 시간이 절약된다. 
    - HashSet, TreeSet은 요소를 분리하기가 쉽지 않고, LinkedList는 랜덤 액세스를 지원하지 않아 링크를 따라가야 하므로 역시 요소를 분리하기가 쉽지 않다. 
    - BufferedReader.lines()은 전체 요소의 수를 알기 어렵기 때문에 포크 단계에서 부분요소로 나누기 어렵다.
    - 따라서 이들은 ArrayList, 배열 보다는 상대적으로 병렬 처리가 늦다.
  - 코어(Core)의 수
    - 싱글 코어 CPU일 경우에는 순차 처리가 빠르다. 
    - 병렬 처리를 할 경우 스레드의 수만 증가하고 번갈아 가면서 스케쥴링을 해야하므로 좋지 못한 결과를 준다. 
    - 코어의 수가 많으면 많을 수록 병렬 작업 처리 속도는 빨라진다.

<br/>

