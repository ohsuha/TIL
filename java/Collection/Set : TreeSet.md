# TreeSet
```java
public class TreeSet<E> extends AbstractSet<E>
    implements NavigableSet<E>, Cloneable, java.io.Serializable {
	private transient NavigableMap<E,Object> m;
}
```
- 이진 검색 트리라는 자료 구조의 형태로 데이터를 저장하는 클래스이다.
- 정열된 위치에 저장하므로 저장순서를 유지하지도 않는다.
- 이진 검색 트리는 부모노드의 왼쪽에는 부모노드의 값보다 작은 값의 자식 노드를, 오른쪽에는 큰 값의 자식 노드를 저장한다.
- TreeSet 에 저장되는 객체가 Comparable 을 구현하던가, TreeSet 에게 Comparator 를 제공해서 두객체를 비교할 방법을 알려주지 않으면 예외가 발생한다.
- 정렬된 상태를 유지하기 때문에 정렬 기능과 단일 값 검색과 범위 검색이 매우 빠르다.
- 단 순차적으로 저장하는 것이 아니라 저장 위치를 찾아서 저장하고 삭제 하는 경우 트리의 일부를 재구성해야하므로 데이터의 추가/삭제 시간이 더 걸린다.
- 문자열의 정렬 순서는 문자의 코드값이 기준이 되므로 공백, 숫자, 대문자, 소문자 순으로 정렬된다.

## 메소드

| 메소드                      | 설명                                                                 |
|-----------------------------|----------------------------------------------------------------------|
| `first()`                   | `TreeSet`의 첫 번째(가장 낮은) 요소를 반환합니다.                      |
| `last()`                    | `TreeSet`의 마지막(가장 높은) 요소를 반환합니다.                      |
| `ceiling(E e)`              | 지정된 요소 이상인 요소 중 가장 낮은 요소를 반환합니다.                |
| `floor(E e)`                | 지정된 요소 이하인 요소 중 가장 높은 요소를 반환합니다.                |
| `higher(E e)`               | 지정된 요소보다 큰 요소 중 가장 낮은 요소를 반환합니다.                |
| `lower(E e)`                | 지정된 요소보다 작은 요소 중 가장 높은 요소를 반환합니다.              |
| `pollFirst()`               | `TreeSet`에서 첫 번째(가장 낮은) 요소를 제거하고 반환합니다.           |
| `pollLast()`                | `TreeSet`에서 마지막(가장 높은) 요소를 제거하고 반환합니다.            |
| `headSet(E toElement)`      | 지정된 요소보다 작은 요소들로 구성된 부분 집합을 반환합니다.            |
| `headSet(E toElement, boolean inclusive)` | 지정된 요소보다 작거나 같거나 작은 요소들로 구성된 부분 집합을 반환합니다. |
| `tailSet(E fromElement)`    | 지정된 요소보다 큰 요소들로 구성된 부분 집합을 반환합니다.            |
| `tailSet(E fromElement, boolean inclusive)` | 지정된 요소보다 크거나 같은 요소들로 구성된 부분 집합을 반환합니다. |
| `subSet(E fromElement, E toElement)` | 지정된 범위 내의 요소들로 구성된 부분 집합을 반환합니다.       |
| `subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)` | 지정된 범위 내에서 포함 여부에 따라 요소들로 구성된 부분 집합을 반환합니다. |
| `descendingSet()`           | 현재 `TreeSet`의 요소를 역순으로 정렬한 `NavigableSet`을 반환합니다.  |
