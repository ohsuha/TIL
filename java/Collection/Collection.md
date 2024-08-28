# Collection
```java
public interface Collection<E> extends Iterable<E> 
```
JDK 1.2 이전까지는 Vector, Hashtable, Properties 와 같은 컬렉션 클래스와,
다수의 데이터를 저장할 수 있는 클래스를 각각 다른 방식으로 처리해야 했으나 컬렉션 프레임웍이 등장하면서 
모든 컬렉션 클래스를 표준화된 방식으로 다룰 수 있도록 체계화 되었다.

## hierarchy
collection 인터페이스를 구현하는 대표적인 3개의 자료구조는 list, set, queue 가 있다.

![img](https://velog.velcdn.com/images/wnguswn7/post/755de9af-1786-4687-96ca-d80fce247af7/image.PNG)
![img](https://dinfree.com/lecture/language/img/java5.png)

## 메소드
| 메소드 | 설명 |
|--------|------|
| `boolean add(E e)` | 지정된 요소를 컬렉션에 추가. |
| `boolean addAll(Collection<? extends E> c)` | 지정된 컬렉션의 모든 요소를 현재 컬렉션에 추가. |
| `void clear()` | 컬렉션의 모든 요소를 제거. |
| `boolean contains(Object o)` | 컬렉션에 지정된 요소가 포함되어 있는지 여부를 반환. |
| `boolean containsAll(Collection<?> c)` | 컬렉션이 지정된 컬렉션의 모든 요소를 포함하고 있는지 여부를 반환. |
| `boolean equals(Object o)` | 지정된 객체가 이 컬렉션과 같은지 비교. |
| `int hashCode()` | 컬렉션의 해시코드를 반환. |
| `boolean isEmpty()` | 컬렉션이 비어 있는지 여부를 반환. |
| `Iterator<E> iterator()` | 컬렉션의 요소에 대한 반복자를 반환. |
| `boolean remove(Object o)` | 컬렉션에서 지정된 요소의 단일 발생을 제거. |
| `boolean removeAll(Collection<?> c)` | 지정된 컬렉션에 포함된 모든 요소를 이 컬렉션에서 제거. |
| `boolean retainAll(Collection<?> c)` | 이 컬렉션에서 지정된 컬렉션에 포함된 요소만 유지. |
| `int size()` | 컬렉션에 포함된 요소의 수를 반환. |
| `Object[] toArray()` | 컬렉션의 모든 요소를 포함하는 배열을 반환. |
| `<T> T[] toArray(T[] a)` | 지정된 배열에 컬렉션의 모든 요소를 저장하여 반환. |
