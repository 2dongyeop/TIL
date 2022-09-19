# 내부 반복자와 외부 반복자
> 본 글은 외부 반복자의 사용은 익숙한 반면에, 내부 반복자에 익숙하지 않은 탓에
> 
> 내부 반복자를 더 알아보고자, 내부 반복자를 중심으로 작성하였습니다.

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/Java/image/iterator.png" width = 500/>

<br/>

## 내부 반복자와 외부 반복자의 차이
- 내부 반복자 : 처리할 행동(보통 콜백 함수)을 컬렉션 요소에 넘겨주어 반복 처리한다.
- 외부 반복자 :  일반적으로 사용하는 루프처럼 요소를 사용하는 측(클라이언트)가 직접 컬렉션 요소를 하나씩 꺼내와서 반복 처리한다.

<br/>

### **내부 반복자의 이점**
- 외부 반복자보다 효율적으로 반복 작업을 수행한다.
    - 요소들을 반복 순서를 변경하거나 , 멀티 코어 CPU를 최대한 활용하기 위해 요소들을 분배시켜 병렬 작업을 할 수 있게 도와준다.
    
<br/>

### **내부 반복자의 장단점**
- 장점 : 집합 객체에 따라 다양한 순회 방법을 제공한다.
- 단점 : 사용자가 직접 반복자를 삭제하는 책임을 가져야 한다.

<br/>

### **일반적인 반복문과의 차이**
- `Iterator`를 사용하면 순회 과정에서 요소를 삭제할 수 있다.
- `for each`문에서는 순회 과정에서 삭제 시 예외 발생
    - 아직 순회하지 않은 자료들의 인덱스가 변경하여 `ConcurrentModificationException` 가 발생한다.
        - 단, 순회를 역순으로 하면서 삭제하면 예외 발생하지 않음
        - 또는 `removeIf()` 사용하기

    <br/>

    > 순회 중 삭제를 했을 경우, 차이를 보여주는 코드
    ```Java
    List<User> userList = getTestUserList();

    // Iterator를 사용한 방법
    for (Iterator<User> iterator = userList.iterator(); iterator.hasNext();) {
        User user = iterator.next();

        if ("gildong".equals(user.getLoginId())) {
            iterator.remove();
        }
    }

    // 향상된 For문을 사용한 방법
    // ConcurrentModificationException Error
    for (User user : userList) {
        if ("gildong".equals(user.getLoginId())) {
            userList.remove(user);
        }
    }

    // 해결법
    userList.removeIf(user -> "gildong".equals(user.getLoginId()));
    ```

<br/>

### 코드로 이해하는 내부 반복 과정
```java
public class InternalIterationList<E> extends ArrayList<E> {
    
    private static final long serialVersionUID = 1L;

    // 반복 작업을 처리하게 할 콜백 인터페이스.
    // 편의상 스테틱 내부 인터페이스로 구현
    public static interface Callback<E> {
        public E map(E e);
    }
    
    // 실제 반복자. 자신의 요소에 콜백을 한번씩 호출
    public void each(Callback<E> callback) {
        int len = this.size();
        for (int i = 0; i < len; i++) {
            callback.map(this.get(i));
        }
    }
    
    @Test
    public void testClass() {
        InternalIterationList<String> internalIterator 
            = new InternalIterationList<String>();
        
        internalIterator.add("Hello");
        internalIterator.add(", ");
        internalIterator.add("World");
        
        // 내부 반복
        internalIterator.each(new Callback<String>() {
            
            @Override
            public String map(String e) {
                System.out.print(e);
                return e;
            }
        });
	}
}
```