# HashMap
```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {
	    static class Node<K,V> implements Map.Entry<K,V> {
			final int hash;
			final K key;
			V value;
			Node<K,V> next;
			
			Node(int hash, K key, V value, Node<K,V> next) {
				this.hash = hash;
				this.key = key;
				this.value = value;
				this.next = next;
			}
		}
	    
    	transient Node<K,V>[] table;
	    transient Set<Map.Entry<K,V>> entrySet;
	    transient int size;
	    transient int modCount;
	    int threshold;
    	final float loadFactor;
        public HashMap(int initialCapacity, float loadFactor) {
            if (initialCapacity < 0)
                throw new IllegalArgumentException("Illegal initial capacity: " +
                    initialCapacity);
            if (initialCapacity > MAXIMUM_CAPACITY)
                initialCapacity = MAXIMUM_CAPACITY;
            if (loadFactor <= 0 || Float.isNaN(loadFactor))
                throw new IllegalArgumentException("Illegal load factor: " +
                    loadFactor);
            this.loadFactor = loadFactor;
            this.threshold = tableSizeFor(initialCapacity);
        }

        static final int tableSizeFor(int cap) {
            int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
            return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
        }
}
```
- HashMap은 Hashtable 을 개선해서 나온 것이다.
  - Hashtable : 
  - 모든 메서드가 동기화되어 멀티스레드 환경에서 안전하게 사용할 수 있다.
  - 동기화 때문에 HashMap보다 성능이 떨어질 수 있다.
  - 저장된 요소의 순서가 보장되지 않는다.
  - null 키와 값을 허용하지 않는다. null을 사용하려 하면 NullPointerException이 발생한다.
- 삽입과 검색이 평균적으로 빠르다. TreeMap 보다 HashMap이 검색 성능이 좋다.
- 요소의 순서를 보장하지 않는다.
- 스레드에 안전한 맵을 만들려면 `Collections.synchronizedMap(new HashMap())` 을 사용해야한다.
- 기본 생성자로 생성할 때 기본 16개의 기본 영역을 만든다. 데이터의 갯수가 많을 경우 초기 크기를 지정하는 것이 권장된다.
- 키가 되는 객체를 직접 작성할 때는 hashCode(), equals() 를 꼭 구현해야한다.
- 존재하지 않는 키를 get()할 경우 null 이 리턴된다.

## 고유 메소드
| 메서드                         | 설명                                                                                  | 반환 타입      |
|--------------------------------|---------------------------------------------------------------------------------------|----------------|
| `void rehash()`                | 해시맵의 모든 항목을 다시 해싱하여 배열의 크기를 조정합니다. (Java 8 이후 사라짐)         | `void`         |
| `V remove(Object key)`         | 주어진 키에 해당하는 항목을 제거합니다.                                                | `V`            |
| `void clear()`                 | 해시맵의 모든 항목을 제거합니다.                                                       | `void`         |
| `boolean containsKey(Object key)` | 주어진 키가 해시맵에 존재하는지 확인합니다.                                          | `boolean`      |
| `boolean containsValue(Object value)` | 주어진 값이 해시맵에 존재하는지 확인합니다.                                   | `boolean`      |
| `boolean isEmpty()`            | 해시맵이 비어 있는지 확인합니다.                                                       | `boolean`      |
| `int size()`                   | 해시맵에 포함된 항목의 개수를 반환합니다.                                              | `int`          |
| `V putIfAbsent(K key, V value)` | 주어진 키가 해시맵에 없으면 키와 값을 추가합니다.                                      | `V`            |
| `V getOrDefault(Object key, V defaultValue)` | 주어진 키에 해당하는 값을 반환하거나 키가 없으면 기본 값을 반환합니다.      | `V`            |
| `void forEach(BiConsumer<? super K,? super V> action)` | 해시맵의 각 항목에 대해 주어진 작업을 수행합니다. | `void`         |
| `void replaceAll(BiFunction<? super K,? super V,? extends V> function)` | 해시맵의 각 값에 대해 주어진 작업을 수행하고 그 결과로 값을 대체합니다. | `void` |
| `V replace(K key, V value)`    | 주어진 키에 해당하는 값을 새로운 값으로 대체합니다.                                    | `V`            |
| `boolean replace(K key, V oldValue, V newValue)` | 주어진 키의 값이 특정 값과 일치할 때만 값을 대체합니다.                      | `boolean`      |
| `V compute(K key, BiFunction<? super K,? super V,? extends V> remappingFunction)` | 주어진 키의 값을 계산하고, 계산 결과로 값을 대체하거나 제거합니다.           | `V`            |
| `V computeIfAbsent(K key, Function<? super K,? extends V> mappingFunction)` | 주어진 키가 없을 경우 값 계산 후 추가합니다.                              | `V`            |
| `V computeIfPresent(K key, BiFunction<? super K,? super V,? extends V> remappingFunction)` | 주어진 키의 값이 존재하면 값 계산 후 대체합니다.                         | `V`            |
| `V merge(K key, V value, BiFunction<? super V,? super V,? extends V> remappingFunction)` | 주어진 키에 대한 값을 결합하고, 결합된 결과로 값을 대체합니다.          | `V`            |
- keyset() : key를 모두 set 타입으로 리턴
- values() : 값을 모두 collection 타입으로 리턴
- entrySet() : 키와 값을 모두 set 타입으로 리턴
- containKey(), containValue() : boolean 타입의 값 리턴



## WeakHashMap
- 키에 대해 약한 참조를 사용하여, 다른 곳에서 참조되지 않는 키는 가비지 컬렉터가 제거할 수 있다.
- 사용되지 않는 객체를 자동으로 수거하여 메모리 사용을 효율적으로 관리할 수 있다.
- 캐시와 같은 곳에서 더 이상 사용되지 않는 데이터를 자동으로 정리하고 싶을 때 유용하다.

## LinkedHashMap
- 입력된 순서를 유지하는 Map
- 삽입 순서나 액세스 순서를 유지하면서도 HashMap과 비슷한 성능을 제공한다. 단 추가적인 메모리가 필요하다.

## 해싱과 해싱 함수

해싱이란 해시함수를 통해서 데이터를 해시테이블에 저장하고 검색하는 기법을 말한다.
데이터가 저장되어 있는 곳을 알려주기 때문에 데이터 중에서도 원하는 데이터를 빠르게 찾을 수 있다.

해싱에서 사용하는 자료구조는 배열과 링크드 리스트의 조합으로 이루어져있다.
저장할 데이터의 키를 해시함수에 넣으면 배열의 한 요소를 얻게 되고, 다시 그 곳에 연결되어있는 링크드 리스트에 저장한다.
배열이 하나 있는데 각각의 배열이 또 링크드 리스트로 되어있는 구조임
1. 검색하고자 하는 값의 키로 해시함수를 호출한다.
2. 해시함수의 계산결과(해시코드)로 해당 값이 저장되어 있는 링크드리스트를 찾는다.
3. 링크드 리스트에서 검색한 키와 일치하는 데이터를 찾는다.

### 이렇게 하는 이유?
- 링크드 리스트는 검색에 불리한 자료구조이기 때문에 크기가 커질수록 검색속도가 떨어진다.
- 반면에 배열은 배열의 크기가 커져도 원하는 요소가 몇 번째에 있는지만 알면 빠르게 찾을 수 있다.
- 해싱을 구현하는 과정에서 해시함수의 알고리즘이 중요하다.
- Object 의 hashCode()를 해시함수로 사용한다.

### 해싱이 hashMap에 어떻게 들어가 있는데?
`HashMap` 클래스에서 해싱이 구현된 부분은 주로 키를 저장할 위치를 결정하는 과정에서 이루어집니다. 이 과정은 해시 함수를 사용하여 키의 해시 코드를 생성하고, 이 해시 코드를 통해 배열 내에서 저장할 위치를 결정하는 방식으로 이루어집니다.

주요 구현은 `HashMap` 클래스의 `put`, `get` 메서드에서 이루어지며, `Node<K,V>[] table` 배열이 해시 맵의 버킷을 저장하는 데 사용됩니다. 아래에 해싱이 어떻게 이루어지는지 관련된 코드를 살펴보겠습니다.

### 1. `hash()` 메서드

`HashMap` 클래스에는 `hash()`라는 정적 메서드가 있는데, 이 메서드는 키 객체의 `hashCode()`를 이용하여 해시 값을 계산합니다. 이 메서드는 해시 코드를 재조정하여 해시 분포가 더 균등해지도록 합니다.

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

- `key.hashCode()`는 객체의 해시 코드를 반환합니다.
- `h ^ (h >>> 16)`는 해시 코드와 그 해시 코드를 16비트 우측 이동한 값의 XOR 연산을 수행하여 해시 충돌을 줄이도록 도와줍니다.

### 2. `put()` 메서드에서의 해싱

`put()` 메서드는 키-값 쌍을 해시 맵에 추가할 때 호출됩니다. 이 과정에서 `hash()` 메서드를 사용하여 키의 해시 값을 계산하고, 이 값을 기반으로 배열 내의 위치를 결정합니다.

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```

여기서 `hash(key)`를 통해 키의 해시 값을 계산하고, `putVal()` 메서드에서 이 해시 값을 사용하여 데이터를 저장할 위치를 결정합니다.

### 3. `get()` 메서드에서의 해싱

`get()` 메서드는 키를 통해 값을 검색할 때 사용됩니다. 이 메서드도 역시 `hash()` 메서드를 통해 키의 해시 값을 계산하고, 이를 기반으로 값이 저장된 위치를 찾습니다.

```java
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
```

여기서도 `hash(key)`를 사용하여 해시 값을 계산하고, `getNode()` 메서드에서 해당 해시 값을 기반으로 올바른 노드를 찾습니다.

### 4. `indexFor()` (Java 8 이전)

Java 8 이전에서는 `indexFor()` 메서드를 사용하여 해시 값을 기반으로 배열 내 인덱스를 계산했습니다. 그러나 Java 8부터는 이 메서드가 사라지고, 대신 `table.length`로 비트를 마스킹하는 방식이 사용됩니다.

```java
static int indexFor(int h, int length) {
    return h & (length-1);
}
```

여기서 `h & (length - 1)`는 해시 값을 배열 크기보다 작은 값으로 변환하는 역할을 합니다.

---

결론적으로, 해싱이 구현된 부분은 `hash()` 메서드를 통해 해시 값을 계산하고, 이를 기반으로 데이터를 저장할 위치를 결정하는 `put()` 및 `get()` 메서드에서 이루어집니다.

