# list 
```java
public interface List<E> extends SequencedCollection<E>
```
- 중복을 허용하면서 저장 순서가 유지되는 컬렉션
- List 인터페이스를 구현한 클래스로는 java.util 의 ArrayList, LinkedList, Vector 가 있다.

## 메소드
| 메소드 | 설명 |
|--------|------|
| `void add(int index, E element)` | 지정된 위치에 요소를 삽입. |
| `E get(int index)` | 지정된 위치에 있는 요소를 반환. |
| `int indexOf(Object o)` | 리스트에서 지정된 요소의 첫 번째 발생 위치를 반환. |
| `int lastIndexOf(Object o)` | 리스트에서 지정된 요소의 마지막 발생 위치를 반환. |
| `ListIterator<E> listIterator()` | 리스트의 요소에 대한 리스트 반복자를 반환. |
| `ListIterator<E> listIterator(int index)` | 지정된 위치에서 시작하는 리스트 반복자를 반환. |
| `E remove(int index)` | 지정된 위치에 있는 요소를 제거하고, 제거된 요소를 반환. |
| `E set(int index, E element)` | 리스트의 지정된 위치에 있는 요소를 지정된 요소로 대체. |
| `List<E> subList(int fromIndex, int toIndex)` | 리스트의 지정된 범위에 대한 부분 리스트를 반환. |
