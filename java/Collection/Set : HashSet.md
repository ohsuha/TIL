# HashSet
```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable{

	public HashSet() {
		map = new HashMap<>();
	}
}
```
- backed by a hash table (actually a HashMap instance).
- this implementation is not synchronized, the set should be "wrapped" using the Collections. synchronizedSet method. This is best done at creation time, to prevent accidental unsynchronized access to the set
- 저장 순서를 유지하지 않는다. 
- add()를 통해서 데이터를 추가할때 중복이 발생하면 false 를 리턴한다.
- 저장 순서를 유지하며 중복을 허용하고 싶지 않을때는 LinkedHashSet 을 사용하자
- 해시 테이블을 사용해서 요소를 저장한다.
- 요소 추가, 제거, 검색이 빠르다.
- 로드팩터를 통해 데이터의 갯수가 증가하여 로드 팩터보다 더 커지면 재 정리 작업을 해야한다.
- 내부의 자료구조를 다시 생성해야해서 비용이 만이 들지만 로드 팩터가 클 경우에는 데이터를 찾는 시간이 증가한다.
    - `로드팩터 = 데이터의 갯수 / 저장공간`

## 왜 내부에 HashMap을 사용할까?
`HashSet`이 `HashMap`을 사용하여 구현되는 이유는 **효율성**과 **단순성** 때문입니다. `HashSet`은 중복을 허용하지 않고, 요소들이 순서 없이 저장되는 자료 구조입니다. 이런 특성을 구현하기 위해서는 내부적으로 요소들의 **고유성**을 빠르게 검사할 수 있는 메커니즘이 필요합니다.

### `HashSet`과 `HashMap`의 관계

- **`HashSet`**: 내부적으로 데이터를 저장하기 위해 `HashMap`을 사용합니다.
- **`HashMap`**: 키-값 쌍으로 데이터를 저장하는 구조입니다. 이때, `HashSet`은 값 대신 키만을 사용하며, 모든 키에 대해 동일한 값을 저장합니다 (일반적으로 `PRESENT` 같은 객체를 사용).

### 왜 `HashMap`을 사용하나요?

1. **중복 없는 데이터 저장**:
    - `HashMap`은 키에 대해 고유성을 보장합니다. 즉, 동일한 키가 다시 추가되면 기존의 값이 업데이트되며, 새로운 키가 추가되지 않습니다. 이 특성은 `HashSet`이 요구하는 중복 없는 데이터 저장과 일치합니다.

2. **상수 시간 복잡도**:
    - `HashMap`은 내부적으로 해시 함수를 사용하여 키를 해시 버킷에 저장하므로, 요소를 추가하거나 검색할 때 평균적으로 O(1) 시간 복잡도를 가집니다. 이는 `HashSet`이 원하는 성능입니다.

3. **단순한 구현**:
    - 이미 효율적인 자료 구조인 `HashMap`이 있으므로, 별도의 구조를 구현하지 않고도 `HashSet`을 손쉽게 만들 수 있습니다. `HashMap`의 키만을 사용하고 값은 무시하여, 단순하면서도 효율적인 `Set` 구현이 가능합니다.

HashMap에서 키들은 Set처럼 고유하게 관리되지만, 내부적으로 Set을 직접 사용하는 것은 아닙니다. 대신 HashMap은 해시 함수와 equals() 메서드를 통해 고유성을 관리하며, 해시 버킷을 통해 키-값 쌍을 저장하는 고유한 방식으로 동작합니다.