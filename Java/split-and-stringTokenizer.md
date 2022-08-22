> 본 포스팅은 아래의 기본 지식을 알고 있다는 전제 하에 작성합니다.
>
> 1. BufferedReader
> 2. Wrapper class
> 3. 입출력 Stream 

<br/>

## Java에는 문자열을 원하는 구분자로 분리하고 싶을때 사용하는 두 가지 방식이 있다.
1. String 클래스의 split() 메소드 
2. java.util에 들어 있는 StringTokenizer

<br/>

### split() 메소드
- 지정한 구분자로 문자열을 나눠 배열에 저장한다.
- 공백 문자열도 포함한다.

<br/>

### StringTokenizer
- 지정한 한 가지 구분자로 문자열을 나눌 수 있다.
- 공백 문자열은 포함하지 않는다.

<br/>

### split() 메소드 사용 예시
```Java
System.out.println("학급 수 N과 학급당 최대 인원수 M을 입력");
String[] str = br.readLine().split(" ");

int N = Integer.parseInt(str[0]);
int M = Integer.parseInt(str[1]);
```
- 동작 설명 : 한 줄에 입력값이 “3 1”로 들어오면, 구분자에 맞춰 `split()` 메서드로 구분한다.
    - 미리 선언한 String 타입의 배열 str에 저장된다.
    - 입력 값은 String 타입이고, 저장하는 변수는 int 타입이므로 `boxing` 과정이 필요하다.
    - 최종적으로 N = 3, M = 1이 저장된다.

<br/>

### StringTokenizer 사용 예시
```java
System.out.println("정점의 개수와 간선의 개수를 입력");
String s = br.readLine();

StringTokenizer st1 = new StringTokenizer(s);

int vCount = Integer.parseInt(st1.nextToken());
int eCount = Integer.parseInt(st1.nextToken());
```

- 동작 설명 : 한 줄에  입력값이 “3 1”로 들어오면, 구분자에 맞춰 StringTokenizer가 분리한다.
    - 위처럼 매개변수에 구분자를 정해주지 않을 경우, s를 공백으로 분리한다.
    - `nextToken()` 메서드를 통해 분리한 내용을 반환하고, 타입이 다를 경우 `boxing` 과정이 필요하다.

<br/>

### 비교 예시

> 모든 구분자를 ,로 가정하여 주어진 문자열의 경우에 따라 나눈다.

<br/>

1. 데이터 + 구분자 + 데이터 형태일 경우
    - ex) String str = “soccer,baseball,basketball”; 
    - split()의 경우
        - soccer
        - baseball
        - basketball
    - StringTokenizer 의 경우
        - soccer
        - baseball
        - basketball

- 결과가 일치한다.

<br/>

2. 데이터 + 구분자 + 구분자 + 데이터 형태일 경우 (중간에 데이터가 없는 경우이다.)
    - ex) String str = “soccer,baseball,,basketbal”; 
    - split()의 경우
        - soccer
        - baseball
        - 
        - basketball
    - StringTokenizer 의 경우
        - soccer
        - baseball
        - basketball

- split()은 공백 문자열도 포함하기 때문에, 결과가 일치하지 않는다.


<br/>

3. 데이터 + 구분자 + 데이터 + 구분자 형태일 경우(구분자가 마지막에 있는 경우)
    - ex) String str = "soccer,baseball,basketball,";
    - split()의 경우
        - soccer
        - baseball
        - basketball
    - StringTokenizer 의 경우
        - soccer
        - baseball
        - basketball

- 두 경우 모두 마지막 구분자는 무시하기 때문에, 결과가 일치한다.

<br/>

## 마무리
속도 측면에서는 StringTokenizer가 split() 메서드보다 우수하다.

→ split() 메서드는 인자로 [정규 표현식](https://hbase.tistory.com/160)을 사용하기 때문이다.