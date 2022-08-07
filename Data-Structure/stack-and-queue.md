# 스택과 큐

## 스택(Stack)이란?
- 선형 자료구조의 일종으로, `LIFO(:Last In First Out)`의 구조를 가진다.
- 여기서 `LIFO`는 후입선출 방식으로, 나중에 들어간 원소가 먼저 나오는 방식이다.
   - 웹 브라우저의 방문 기록(뒤로가기), 실행 취소(undo) 등에 사용된다.
- 시간 복잡도 : O(n), 공간 복잡도 : O(n)

<br/>

## 큐(Queue)란?
- 선형 자료구조의 일종으로, `FIFO(:First In First Out)`의 구조를 가진다.
- 여기서 `FIFO`는 선입선출 방식으로, 가장 먼저 들어간 원소가 먼저 나오는 방식이다.
    - 프린터의 인쇄 대기열, BFS, 버퍼(Buffer), 메시지 큐 등에 사용된다.
- 시간 복잡도 : O(n), 공간 복잡도 : O(n)
- Java에는 Queue 인터페이스가 있어, 이를 구현하는 PriorityQueue 등을 사용하곤 한다.

<br/>

## 스택을 이용하여 큐를 구현하는 방법
<img src="https://github.com/2dongyeop/TIL/blob/main/Data-Structure/image/stack-make-queue.png" width = 500/>

### 동작 방식 
1. inbox에 데이터를 삽입한다(push) -> A,B
2. inbox에 있는 데이터를 추출(pop)하여 outbox에 삽입(push) 한다. -> B,A
3. outbox에 있는 데이터를 추출(pop)한다. -> A,B 순으로 출력된다.

<br/>

> 즉, inbox 스택에 A,B 순으로 데이터를 삽입하면 위의 그림처럼 스택에 데이터가 쌓인다. 
>
> 그리고 inbox 스택에 있는 데이터를 pop해서 outbox로 옮긴다. 
>
> 그렇게 되면 outbox에는 B,A 순서로 데이터가 쌓인다.
>
> 그리고 다시 outbox 스택에 있는 데이터를 pop하게 되면 A,B 순으로 데이터가 출력된다.

<br/>

### Java로 구현한 코드
```Java
import java.util.Stack;

/**
 * created by victory_woo on 2020/05/06
 * Stack 2개를 이용해서 Queue 구현하기.
 */
public class StackWithQueue {
    public static void main(String[] args) {
        StackQueue<String> queue = new StackQueue<>();
        queue.enQueue("A");
        queue.enQueue("B");
        queue.enQueue("C");

        System.out.println(queue.deQueue());
        System.out.println(queue.deQueue());
        System.out.println(queue.deQueue());
    }

    static class StackQueue<T> {
        private Stack<T> inBox;
        private Stack<T> outBox;

        StackQueue() {
            inBox = new Stack<>();
            outBox = new Stack<>();
        }

        void enQueue(T item) {
            inBox.push(item);
        }

        Object deQueue() {
            if (outBox.isEmpty()) {
                while (!inBox.isEmpty()) {
                    outBox.push(inBox.pop());
                }
            }

            return outBox.pop();
        }
    }
}

```
