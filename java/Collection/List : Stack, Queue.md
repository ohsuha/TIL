## Vector 를 구현한 Stack
```java
public class Stack<E> extends Vector<E>
```
- LIFO 를 위해 만들어졌지만 이것보다 빠른 ArrayDeque(스레드에 안전하지 x) 가 있다.
- 사용 예시 : 수식계산, 수식 괄호 검사, 웹브라우저의 뒤로/앞으로, undo/redo

- | 메소드명 | 설명 |
  |----------|------|
  | `peek()` | 스택의 가장 위에 있는 데이터를 리턴만 합니다. |
  | `pop()`  | 스택의 가장 위에 있는 데이터를 리턴하고 지웁니다. |
  | `push(E item)` | 매개변수로 넘어온 데이터를 스택의 가장 위에 저장합니다. |
  | `search(Object o)` | 매개변수로 들어온 데이터의 위치를 리턴합니다. |


## Queue 를 구현한 LinkedList
- FIFO 를 위해 만들어졌다.
- 한쪽 끝으로만 추가/삭제할 수 있다.
- 사용 예시 : 최근 사용문서, 인쇄작업 대기 목록, 버퍼

### PriorityQueue
```java
public class PriorityQueue<E> extends AbstractQueue<E>
    implements java.io.Serializable
```
- 저장 순서에 관계없이 우선순위가 높은것 부터 꺼내는 것이 특징이다.
- 저장공간으로 배열을 사용하며 각 요소를 heap 이라는 자료구조의 형태로 저장한다.
  - 힙은 이진트리의 한 종류이다.
- null 을 저장할 수 없다.

### Deque
```java
public interface Deque<E> extends Queue<E>, SequencedCollection<E>

<impl>
public class ArrayDeque<E> extends AbstractCollection<E>
	implements Deque<E>, Cloneable, Serializable

public class LinkedList<E>
	extends AbstractSequentialList<E>
	implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```
- 양쪽 끝으로 추가/삭제가 가능하다.
- 스택으로사용할수도 있고 큐로도 사용할 수 있다.