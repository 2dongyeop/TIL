# Call by Value와 Call by Reference

### Call by Value(값에 의한 호출)이란?
- call by value 호출 방식은 함수 호출 시 전달되는 변수 값을 복사해서 함수 인자로 전달한다.
  - 이때 복사된 인자는 함수 안에서 지역적으로 사용되기 때문에 local 속성을 가진다.
  - 따라서, 함수 안에서 인자 값이 변경되더라도, 외부 변수 값은 영향을 받지 않는다.

<br/>

> 즉, 쉽게 말해 메소드 호출 시에 사용되는 인자의 메모리에 저장되어 있는 값(value)을 복사하여 보낸다.
> 
> 따라서 객체의 주소가 아닌, 내부 값을 받아 처리하는 방식이다.


<br/>

- C언어로 작성한 예시 코드
```c
void func(int n) {
    n = 20;
}

void main() {
    int n = 10;
    func(n);
    printf("%d", n);
}
```

<br/>

- Java로 작성한 예시 코드
```java
public class CallByValue {
    private static void swapS(String one, String two) {
        String temp = one;
        one = two;
        two = temp;
    }
    
    public static void main(String args[]) {
        String a = new String("안녕");
        String b = new String("잘가");

        System.out.println("Before");
        System.out.println(a+b);            // 안녕잘가
        swapS(a,b);
        System.out.println("After");
        System.out.println(a+b);            // 안녕잘가
    }
}
```


<br/>

### Call by Reference(참조에 의한 호출)이란?
- call by reference 호출 방식은 함수 호출 시 인자로 전달되는 변수의 레퍼런스를 전달한다.
  - 따라서 함수 안에서 인자 값이 변경되면, 아규먼트로 전달된 객체의 값도 변경된다.

<br/>

> 단, Java의 경우 항상 call by value로 값을 넘긴다.
> 
> C/C++와 같이 변수의 주소값 자체를 가져올 방법이 없으며, 이를 넘길 수 있는 방법 또한 있지 않다.
>
> 참조 타입을 넘길 시에는 해당 객체의 주소값을 복사하여 이를 가지고 사용한다.
>
>따라서 **원본 객체의 프로퍼티까지는 접근이 가능**하나, 원본 객체 자체를 변경할 수는 없다.


<br/>

- C언어로 작성한 예시 코드
```C
void func(int *n) {
    *n = 20;
}

void main() {
    int n = 10;
    func(&n);
    printf("%d", n);
}
```

<br/>

- Java로 흉내낸 예시 코드
```Java
public class CallByReference {
    int value;
    
    CallByReference(int value) {
        this.value = value;
    }
    
    private static void swap(CallByReference one, CallByReference two) {
        int temp = one.value;
        one.value = two.value;
        two.value = temp;
    }
    
    public static void main(String[] args) {
        CallByReference a = new CallByReference(10);
        CallByReference b = new CallByReference(20);

        System.out.println("swap() 호출 전 : a = " + a.value + ", b = " + b.value);
        swap(a,b);
        System.out.println("swap() 호출 후 : a = " + a.value + ", b = " + b.value);
    }
}
```

<br/>

---
### Reference
- [Gyoogle](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/%5Bjava%5D%20Call%20by%20value%EC%99%80%20Call%20by%20reference.md)
- [NKLCWDT](https://github.com/NKLCWDT/cs/blob/main/Java/call%20by%20value%2C%20call%20by%20reference.md)