Arrays.sort() 를 호출해서 배열을 정리하는 것은 사실 Character 클래스의 Comparable 의 구현에 의해 정렬되었던 것이다.

컬렉션을 정렬하는데 필요한 메서드를 정의하고 있다.

Comparable 은 같은 타입의 인스턴스끼리 서로 비교할 수 있는 클래스들이며 기본적으로 작은 값에서 큰값의 순으로 정렬되도록 구현되어있다.
Comparable 을 구현한 클래스는 정렬이 가능하다는 것을 의미한다.

Comporator 는 내림차순으로 정렬한다던가 아니면 다른 기준에 의해 정렬되도록 하고 싶을때 구현해서 사용한다.


`Comparable`과 `Comparator`는 Java에서 객체의 정렬 순서를 정의할 때 사용하는 두 가지 주요 인터페이스입니다. 이 인터페이스들은 Java Collections Framework의 중요한 부분으로, 객체들을 정렬하거나 비교하는 방법을 정의합니다.

### `Comparable` 인터페이스

- **목적**: 객체 자신이 자신의 자연 정렬 순서를 정의합니다.
- **위치**: `java.lang` 패키지.
- **주요 메소드**:
    - `int compareTo(T o)`: 현재 객체와 지정된 객체 `o`를 비교하여 정렬 순서를 결정합니다. 반환값이 양수, 음수, 또는 0에 따라 객체의 순서가 정해집니다.
        - **양수**: 현재 객체가 `o`보다 뒤에 있어야 한다는 것을 의미.
        - **음수**: 현재 객체가 `o`보다 앞에 있어야 한다는 것을 의미.
        - **0**: 두 객체가 동등하다는 것을 의미.
- **용도**: 클래스의 자연 정렬 순서를 정의하고자 할 때 사용합니다. 예를 들어, `String`, `Integer`, `Double` 등은 `Comparable`을 구현하여 자연 정렬 순서를 제공합니다.

**예제 코드**:
```java
public class Person implements Comparable<Person> {
    private String name;
    private int age;

    // 생성자, getters, setters 생략

    @Override
    public int compareTo(Person other) {
        return Integer.compare(this.age, other.age); // 나이로 비교
    }
}
```

### `Comparator` 인터페이스

- **목적**: 객체의 정렬 순서를 외부에서 정의합니다. 객체 자체는 정렬 순서를 정의하지 않습니다.
- **위치**: `java.util` 패키지.
- **주요 메소드**:
    - `int compare(T o1, T o2)`: 두 객체 `o1`과 `o2`를 비교하여 정렬 순서를 결정합니다. 반환값이 양수, 음수, 또는 0에 따라 객체의 순서가 정해집니다.
        - **양수**: `o1`이 `o2`보다 뒤에 있어야 한다는 것을 의미.
        - **음수**: `o1`이 `o2`보다 앞에 있어야 한다는 것을 의미.
        - **0**: 두 객체가 동등하다는 것을 의미.
- **용도**: 클래스의 자연 정렬 순서와는 다른 여러 정렬 기준을 제공하고자 할 때 사용합니다. 예를 들어, 나이, 이름 등 다양한 기준으로 객체를 정렬할 수 있습니다.

**예제 코드**:
```java
import java.util.Comparator;

public class PersonAgeComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return Integer.compare(p1.getAge(), p2.getAge()); // 나이로 비교
    }
}
```

### `Comparable`과 `Comparator`의 차이점

- **자연 정렬 순서 vs 외부 정렬 순서**:
    - `Comparable`은 객체 자신의 자연 정렬 순서를 정의합니다.
    - `Comparator`는 객체의 정렬 순서를 외부에서 정의합니다.

- **단일 정렬 기준 vs 다중 정렬 기준**:
    - `Comparable`을 사용하면 객체 자체가 하나의 정렬 기준을 가집니다.
    - `Comparator`를 사용하면 여러 가지 정렬 기준을 정의할 수 있습니다. 예를 들어, 같은 `Person` 객체를 나이, 이름, 또는 다른 기준으로 정렬할 수 있습니다.

- **수정 가능성**:
    - `Comparable`은 클래스가 한 번 정렬 순서를 정의하면 변경할 수 없습니다.
    - `Comparator`는 클래스와 독립적으로 정의되기 때문에 필요에 따라 여러 개를 만들 수 있습니다.

이 두 인터페이스를 사용하면 Java의 컬렉션 프레임워크에서 객체를 정렬하는 방법을 유연하게 제어할 수 있습니다.

