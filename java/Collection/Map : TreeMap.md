# TreeMap
```java
public class TreeMap<K,V>
    extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable
{
	public TreeMap(Map<? extends K, ? extends V> m) {
		comparator = null;
		putAll(m);
	}

	static final class Entry<K,V> implements Map.Entry<K,V> {
		K key;
		V value;
		Entry<K, V> left;
		Entry<K, V> right;
		Entry<K, V> parent;
		boolean color = BLACK;

		Entry(K key, V value, Entry<K,V> parent) {
			this.key = key;
			this.value = value;
			this.parent = parent;
		}
	}

	private class SubMap extends AbstractMap<K,V>
		implements SortedMap<K,V>, java.io.Serializable {
		
	}
	
}
```
- 요소를 정렬된 순서로 저장하는 Map, 키의 자연 순서나 지정된 Comparator에 따라 요소가 정렬됨
- 삽입과 검색이 느리지만 부분 검색이나 정렬에 효율적이다.