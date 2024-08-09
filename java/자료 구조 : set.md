# set
> set은 순서와 상관 없는 여러개의 데이터를 하나의 객체에 넣을 때 사용한다. 데이터가 있는지 없는지 여부 판단에 주로 쓰인다.

## HashSet
- 해시 테이블을 사용해서 요소를 저장한다.
- 요소 추가, 제거, 검색이 빠르다.
- 순서가 유지되지 않고 중복된 값은 저장되지 않는다.

```
import java.util.HashSet;
import java.util.Set;

public class SetExample {
public static void main(String[] args) {
Set<String> set = new HashSet<>();

        // 요소 추가
        set.add("Apple");
        set.add("Banana");
        set.add("Cherry");
        set.add("Apple"); // 중복된 요소는 무시돼

        // Set 출력
        System.out.println("Set: " + set);

        // 요소 포함 여부 확인
        boolean hasApple = set.contains("Apple");
        System.out.println("Contains 'Apple': " + hasApple);

        // 요소 제거
        set.remove("Banana");
        System.out.println("Set after removal: " + set);
    }
}
```

## LinkedHashSet
- 요소의 삽입 순서를 유지하면서도 HashSet과 비슷한 성능을 제공한다.
- 추가적인 메모리를 사용한다.

## TreeSet
- 요소를 정렬된 상태로 저장하고 정렬된 순서로 접근이 가능하다.
- tree 구조는 정렬된 상태를 유지하기 때문에 단일 값 검색과 범위 검색이 매우 빠르다.
- linked list 보다 데이터의 추가 삭제 시간은 더 걸리지만 검색과 정렬 기능이 더 뛰어나다.