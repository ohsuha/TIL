모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스다.
Enumertaion 은 Iterator 의 구버전이며 ListIterator는 Iterator 의 기능을 향상시킨 것이다.

## Iterator
```java
public interface Iterator<E> 
```
컬렉션에 저장된 요소들을 읽어오는 방법을 표준화 했다.
컬렉션은 Iterator 인터페이스를 정의하고, Collection 에서는 Iterator 를 반환하는 iterator()를 정의했다.
List, Set 에서는 다시 각 컬렉션 특징에 알맞게 작성되어있다.

```java
import java.util.ArrayList;
import java.util.Iterator;

Collection c = new ArrayList(); // 여기에 HashSet() 이나 LinkedList() 같은 다른 구현체를 넣을 수 있다.
Iterator it = c.iterator();

while(it.hasNext()) {
	it.next();
    }
```

### 메소드
| 메소드명      | 설명 |
|---------------|------|
| `hasNext()`   | 컬렉션에 다음 요소가 있는지 여부를 확인합니다. 다음 요소가 있으면 `true`, 없으면 `false`를 반환합니다. |
| `next()`      | 컬렉션의 다음 요소를 반환하고, 반복자의 현재 위치를 다음으로 이동시킵니다. 다음 요소가 없으면 `NoSuchElementException`을 발생시킵니다. |
| `remove()`    | 현재 요소를 컬렉션에서 제거합니다. `next()` 메소드를 호출한 후에만 호출할 수 있습니다. 호출하지 않으면 `IllegalStateException`을 발생시킵니다. |


## ListIterator 
```java
public interface ListIterator<E> extends Iterator<E> 
```
- Iterator 를 상속받아 기능을 추가한 것이다.
- Iterator가 단방향 next() 로만 이동할 수 있는 대신 previous() 를 제공해서 양방향 이동이 가능하다. 
- ArrayList, LinkedList 와 같이 List 인터페이스에서만 사용할 수 있다.

| 메소드명            | 설명 |
|---------------------|------|
| `hasNext()`         | 리스트에 다음 요소가 있는지 여부를 확인합니다. 다음 요소가 있으면 `true`, 없으면 `false`를 반환합니다. |
| `next()`            | 리스트의 다음 요소를 반환하고, 반복자의 현재 위치를 다음으로 이동시킵니다. 다음 요소가 없으면 `NoSuchElementException`을 발생시킵니다. |
| `hasPrevious()`     | 리스트에 이전 요소가 있는지 여부를 확인합니다. 이전 요소가 있으면 `true`, 없으면 `false`를 반환합니다. |
| `previous()`        | 리스트의 이전 요소를 반환하고, 반복자의 현재 위치를 이전으로 이동시킵니다. 이전 요소가 없으면 `NoSuchElementException`을 발생시킵니다. |
| `nextIndex()`       | 다음 요소의 인덱스를 반환합니다. 다음 요소가 없으면 리스트의 크기를 반환합니다. |
| `previousIndex()`   | 이전 요소의 인덱스를 반환합니다. 이전 요소가 없으면 -1을 반환합니다. |
| `remove()`          | 현재 요소를 리스트에서 제거합니다. `next()` 또는 `previous()` 메소드를 호출한 후에만 호출할 수 있습니다. 호출하지 않으면 `IllegalStateException`을 발생시킵니다. |
| `set(E e)`          | 현재 요소를 주어진 요소로 교체합니다. `next()` 또는 `previous()` 메소드를 호출한 후에만 호출할 수 있습니다. 호출하지 않으면 `IllegalStateException`을 발생시킵니다. |
| `add(E e)`          | 현재 위치에 주어진 요소를 추가합니다. `next()` 또는 `previous()` 메소드를 호출한 후에만 호출할 수 있습니다. 호출하지 않으면 `IllegalStateException`을 발생시킵니다. |


## Enumeration
Enumeration은 Java에서 오래된 컬렉션 프레임워크의 일부로, 주로 레거시 코드에서 사용됩니다.

| 메소드명            | 설명 |
|---------------------|------|
| `hasMoreElements()` | 컬렉션에 더 많은 요소가 있는지 여부를 확인합니다. 다음 요소가 있으면 `true`, 없으면 `false`를 반환합니다. |
| `nextElement()`     | 다음 요소를 반환합니다. 다음 요소가 없으면 `NoSuchElementException`을 발생시킵니다. |
