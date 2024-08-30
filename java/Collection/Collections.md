# Collections
컬렉션과 관련된 메서드들을 제공한다.
fill(), copy(), sort(), binarySearch() 는 Arrays와 동일하게 제공한다.

## 컬렉션의 동기화
멀티 스레드 프로그래밍에서 데이터의 일관성을 유지하기 위해 동기화가 필요하다.<br>
Vector, hashtable 외에는 동기화가 없으나 필요한 경우 다음의 synchronized로 시작되는 메서드를 이용해서 동기화 처리가 가능하도록 변경 하였다.

| 메서드                                           | 설명                                                        | 사용 예제                                                 |
|--------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------|
| `synchronizedList(List<T> list)`                 | 주어진 리스트를 동기화된 리스트로 래핑합니다.                | `List<String> syncList = Collections.synchronizedList(new ArrayList<>());` |
| `synchronizedMap(Map<K, V> map)`                 | 주어진 맵을 동기화된 맵으로 래핑합니다.                      | `Map<String, Integer> syncMap = Collections.synchronizedMap(new HashMap<>());` |
| `synchronizedSet(Set<T> set)`                    | 주어진 세트를 동기화된 세트로 래핑합니다.                   | `Set<String> syncSet = Collections.synchronizedSet(new HashSet<>());` |
| `synchronizedSortedMap(SortedMap<K, V> map)`     | 주어진 정렬된 맵을 동기화된 정렬된 맵으로 래핑합니다.        | `SortedMap<String, Integer> syncSortedMap = Collections.synchronizedSortedMap(new TreeMap<>());` |
| `synchronizedSortedSet(SortedSet<T> set)`        | 주어진 정렬된 세트를 동기화된 정렬된 세트로 래핑합니다.      | `SortedSet<String> syncSortedSet = Collections.synchronizedSortedSet(new TreeSet<>());` |

## 변경 불가 컬렉션 만들기
데이터보호를 위해 컬렉션을 읽기 전용으로만 만들떄가 있다.<br>
멀티 스레드 프로그래밍에서 여러 스레드가 하나의 컬렉션을 공유하다보면 데이터가 손상될 수 있는데,
이를 방지하기 위함이다. <br>
필요한 경우 unmodifiable 로 시작하는 메서드들을 사용해서 읽기 전용 데이터로 바꿀 수 있다.

| 메서드                                           | 설명                                                        | 사용 예제                                                 |
|--------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------|
| `unmodifiableCollection(Collection<? extends T> c)` | 주어진 컬렉션을 수정할 수 없는 컬렉션으로 래핑합니다.        | `Collection<String> unmodifiableColl = Collections.unmodifiableCollection(new ArrayList<>());` |
| `unmodifiableList(List<? extends T> list)`       | 주어진 리스트를 수정할 수 없는 리스트로 래핑합니다.          | `List<String> unmodifiableList = Collections.unmodifiableList(new ArrayList<>());` |
| `unmodifiableMap(Map<? extends K, ? extends V> m)` | 주어진 맵을 수정할 수 없는 맵으로 래핑합니다.               | `Map<String, Integer> unmodifiableMap = Collections.unmodifiableMap(new HashMap<>());` |
| `unmodifiableSet(Set<? extends T> s)`            | 주어진 세트를 수정할 수 없는 세트로 래핑합니다.             | `Set<String> unmodifiableSet = Collections.unmodifiableSet(new HashSet<>());` |
| `unmodifiableSortedMap(SortedMap<K, ? extends V> m)` | 주어진 정렬된 맵을 수정할 수 없는 정렬된 맵으로 래핑합니다. | `SortedMap<String, Integer> unmodifiableSortedMap = Collections.unmodifiableSortedMap(new TreeMap<>());` |
| `unmodifiableSortedSet(SortedSet<? extends T> s)`  | 주어진 정렬된 세트를 수정할 수 없는 정렬된 세트로 래핑합니다. | `SortedSet<String> unmodifiableSortedSet = Collections.unmodifiableSortedSet(new TreeSet<>());` |


## 싱글톤 컬렉션으로 만들기
매개변수로 저장할 요소를 지정하면 해당하는 요소를 저장하는 컬렉션을 만든다. 반환된 컬렉션은 변경할 수 없다.

| 메서드                                           | 설명                                                        | 사용 예제                                                 |
|--------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------|
| `singleton(T o)`                                | 단일 요소를 가지는 불변 집합을 생성합니다.                   | `Set<String> singletonSet = Collections.singleton("element");` |
| `singletonList(T o)`                            | 단일 요소를 가지는 불변 리스트를 생성합니다.                | `List<String> singletonList = Collections.singletonList("element");` |
| `singletonMap(K key, V value)`                  | 단일 키-값 쌍을 가지는 불변 맵을 생성합니다.                  | `Map<String, Integer> singletonMap = Collections.singletonMap("key", 1);` |


## 한종류의 객체만 저장하는 컬렉션 만들기
컬렉션에 지정된 종류의 객체만 저장할 수 있도록 제한 하고 싶을때 사용한다.

| 메서드                                           | 설명                                                        | 사용 예제                                                 |
|--------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------|
| `checkedCollection(Collection<T> c, Class<T> type)` | 주어진 컬렉션을 지정된 타입으로 체크합니다.                | `Collection<String> checkedColl = Collections.checkedCollection(new ArrayList<>(), String.class);` |
| `checkedList(List<T> list, Class<T> type)`       | 주어진 리스트를 지정된 타입으로 체크합니다.                 | `List<String> checkedList = Collections.checkedList(new ArrayList<>(), String.class);` |
| `checkedMap(Map<K, V> map, Class<K> keyType, Class<V> valueType)` | 주어진 맵의 키와 값을 지정된 타입으로 체크합니다.        | `Map<String, Integer> checkedMap = Collections.checkedMap(new HashMap<>(), String.class, Integer.class);` |
| `checkedSet(Set<T> s, Class<T> type)`            | 주어진 세트를 지정된 타입으로 체크합니다.                   | `Set<String> checkedSet = Collections.checkedSet(new HashSet<>(), String.class);` |
| `checkedSortedMap(SortedMap<K, V> m, Class<K> keyType, Class<V> valueType)` | 주어진 정렬된 맵의 키와 값을 지정된 타입으로 체크합니다. | `SortedMap<String, Integer> checkedSortedMap = Collections.checkedSortedMap(new TreeMap<>(), String.class, Integer.class);` |
| `checkedSortedSet(SortedSet<T> s, Class<T> type)`  | 주어진 정렬된 세트를 지정된 타입으로 체크합니다.           | `SortedSet<String> checkedSortedSet = Collections.checkedSortedSet(new TreeSet<>(), String.class);` |


## 이 밖의 메소드들
| 메서드                                               | 설명                                                        | 사용 예제                                                 |
|------------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------|
| `addAll(Collection<? super T> c, T... elements)`    | 주어진 컬렉션에 여러 요소를 추가합니다.                     | `Collections.addAll(list, "element1", "element2");`      |
| `binarySearch(List<? extends Comparable<? super T>> list, T key)` | 정렬된 리스트에서 이진 검색을 수행하여 주어진 요소의 인덱스를 찾습니다. | `int index = Collections.binarySearch(list, "key");`    |
| `fill(List<? super T> list, T obj)`                 | 리스트의 모든 요소를 주어진 객체로 채웁니다.                | `Collections.fill(list, "default");`                     |
| `frequency(Collection<?> c, Object o)`             | 주어진 컬렉션에서 특정 객체의 발생 빈도를 반환합니다.      | `int freq = Collections.frequency(list, "element");`     |
| `nCopies(int n, T o)`                              | 주어진 객체를 지정된 횟수만큼 반복하여 불변 리스트를 생성합니다. | `List<String> copies = Collections.nCopies(3, "element");` |
| `reverse(List<?> list)`                            | 리스트의 요소 순서를 반전시킵니다.                           | `Collections.reverse(list);`                            |
| `reverseOrder()`                                   | 역순 정렬의 비교자를 반환합니다.                            | `Comparator<String> reverseComparator = Collections.reverseOrder();` |
| `shuffle(List<?> list)`                            | 리스트의 요소 순서를 무작위로 섞습니다.                     | `Collections.shuffle(list);`                             |
| `sort(List<T> list, Comparator<? super T> c)`      | 리스트의 요소를 주어진 비교자를 사용하여 정렬합니다.         | `Collections.sort(list, comparator);`                    |
| `swap(List<?> list, int i, int j)`                  | 리스트의 두 요소를 교환합니다.                              | `Collections.swap(list, 0, 1);`                          |
| `synchronizedCollection(Collection<T> c)`           | 주어진 컬렉션을 동기화된 컬렉션으로 래핑합니다.              | `Collection<String> syncCollection = Collections.synchronizedCollection(new ArrayList<>());` |
