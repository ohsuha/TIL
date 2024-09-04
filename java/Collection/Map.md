# Map
- 키와 값을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는 데 사용된다.
- 키는 중복될 수 없지만 값은 중복을 허용한다.
- 기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남게 된다.
- Hashtable, HashMap, LinkedHashMap, SortedMap, TreeMap 등이 있다.

## Map.Entry 인터페이스
- Map 인터페이스의 내부 인터페이스이다.
- key-value 쌍을 다루기 위해 내부적으로 Entry 인터페이스를 정의해 놓았다.

## 메소드
### Map
| 메소드 | 설명 |
|--------|------|
| `void clear()` | Map의 모든 키-값 쌍을 제거. |
| `boolean containsKey(Object key)` | 지정된 키가 Map에 포함되어 있는지 여부를 반환. |
| `boolean containsValue(Object value)` | 지정된 값이 하나 이상의 키에 매핑되어 있는지 여부를 반환. |
| `Set<Map.Entry<K,V>> entrySet()` | Map에 포함된 모든 키-값 쌍을 `Set`으로 반환. |
| `V get(Object key)` | 지정된 키에 매핑된 값을 반환. |
| `boolean isEmpty()` | Map이 비어 있는지 여부를 반환. |
| `Set<K> keySet()` | Map에 포함된 모든 키를 `Set`으로 반환. |
| `V put(K key, V value)` | 지정된 키와 값을 Map에 추가, 또는 기존 키에 새로운 값을 매핑. |
| `void putAll(Map<? extends K,? extends V> m)` | 지정된 Map의 모든 키-값 쌍을 이 Map에 복사. |
| `V remove(Object key)` | 지정된 키에 매핑된 키-값 쌍을 제거하고, 제거된 값을 반환. |
| `int size()` | Map에 포함된 키-값 쌍의 수를 반환. |
| `Collection<V> values()` | Map에 포함된 모든 값을 `Collection`으로 반환. |

### Map.Entry

| 메소드 | 설명 |
|--------|------|
| `K getKey()` | 이 Map.Entry에서 대응되는 키를 반환. |
| `V getValue()` | 이 Map.Entry에서 대응되는 값을 반환. |
| `V setValue(V value)` | 이 Map.Entry에서 대응되는 값을 지정된 값으로 대체. 대체된 이전의 값을 반환. |
| `boolean equals(Object o)` | 지정된 객체가 이 Map.Entry와 같은지 비교. |
| `int hashCode()` | 이 Map.Entry의 해시코드를 반환. |

### Hash

