# ENUM
> 사실 열거형은 상수 하나하나가 객체이다. 

서로 관련된 상수를 편리하게 선언하기 위한 것으로 여러 상수를 정의할 때 사용하면 유용하다.<br>
열거형이 갖는 값뿐만 아니라 타입도 관리하기 때문에 보다 논리적인 오류를 줄일 수 있다.<br>
자바의 열거형은 타입에 안전한 열거형이라서 값뿐만 아니라 타입까지 체크하기 때문에 타입에 안전하다.<br>
상수의 값이 바뀌어도 기존의 소스를 다시 컴파일 하지 않아도 된다.<br>

```java
enum Direction {EAST, SOUTH, WEST, NORTH}
```
- 열거형 상수간의 비교에는 `==` 을 사용할 수 있다. 단 비교 연산자는 사용할 수 없고 compareTo()는 사용가능하다.
  - 상수 하나하나가 static final 객체이기 때문에 주소값을 비교하는 `==` 연산자로 비교가 가능한 것이다.
- switch 문의 조건식에도 사용할 수 있다.

## 모든 enum 의 조상 java.lang.Enum
```java
public abstract class Enum<E extends Enum<E>>
        implements Constable, Comparable<E>, Serializable {
    private final String name;
    public final String name() {
        return name;
    }
    private final int ordinal;
    public final int ordinal() {
        return ordinal;
    }
    protected Enum(String name, int ordinal) {
        this.name = name;
        this.ordinal = ordinal;
    }
    public String toString() {
        return name;
    }
    public final boolean equals(Object other) {
        return this==other;
    }
    ...
}
```

### method
- `ordinal()` 은 모든 열거형의 조상인 Enum 클래스에 정의된 것으로, 열거형 상수가 정의된 순서(0에서 시작)을 정수로 반환한다.
- `static E values()` 는 열거형의 모든 상수를 배열에 담아 반환한다. 컴파일러가 자동적으로 추가해주는 메서드이다.
- `static E valueOf(String name)` 은 열거형 상수의 이름으로 문자열 상수에 대한 참조르 얻을 수 있게 해준다.
  - `Direction d = Direction.valueOf("EAST");`

## 열거형에 멤버 추가하기
- 열거형 상수의 값이 불연속적인 경우 열거형 상수 이름 옆에 원하는 값을 괄호()와 함께 적어주면 된다.
- 지정된 값을 저장할 수 있는 인스턴스 변수와 생성자를 새로 추가해 주어야한다.
- 모든 열거형 상수에 정의한다음 다른 멤버들을 추가해야한다.
- 열거형의 생성자는 private 이며 변경할 수 없다.

## 열거형에 추상 메서드 추가하기
- 각 열거형 상수가 반드시 이를 구현해야 한다.
```java
public enum Operation {
    ADD {
        @Override
        public int apply(int x, int y) {
            return x + y;
        }
    },
    SUBTRACT {
        @Override
        public int apply(int x, int y) {
            return x - y;
        }
    },
    MULTIPLY {
        @Override
        public int apply(int x, int y) {
            return x * y;
        }
    },
    DIVIDE {
        @Override
        public int apply(int x, int y) {
            return x / y;
        }
    };

    // 추상 메서드 선언
    public abstract int apply(int x, int y);
}

public class Main {
    public static void main(String[] args) {
        int x = 10;
        int y = 5;

        System.out.println("Addition: " + Operation.ADD.apply(x, y));        // 15
        System.out.println("Subtraction: " + Operation.SUBTRACT.apply(x, y)); // 5
        System.out.println("Multiplication: " + Operation.MULTIPLY.apply(x, y)); // 50
        System.out.println("Division: " + Operation.DIVIDE.apply(x, y));     // 2
    }
}
```

## 타입 안전 열거형 패턴
**타입 안전 열거형 패턴**은 Java에서 안전한 방식으로 열거형을 구현하기 위한 디자인 패턴입니다. Java 1.5 이전에는 언어 차원에서 열거형(enum)을 지원하지 않았기 때문에, 열거형을 구현하려면 정수 상수를 사용하거나 타입 안전 열거형 패턴을 사용해야 했습니다.

### 전통적인 정수 상수 열거형의 문제점

```java
public class Direction {
    public static final int NORTH = 0;
    public static final int SOUTH = 1;
    public static final int EAST = 2;
    public static final int WEST = 3;
}
```

- **타입 안전성 부족**: 정수 상수를 사용하면 잘못된 값이 전달될 위험이 큽니다. 예를 들어, 방향을 나타내는 메서드에 잘못된 값인 4가 전달될 수 있습니다.
- **캡슐화 부족**: 정수 상수는 특정 클래스에 종속되지 않으므로, 다른 곳에서도 사용할 수 있어 코드의 의미가 모호해질 수 있습니다.
- **확장성 제한**: 정수 상수는 새로운 값을 추가하거나 기능을 확장하는 데 한계가 있습니다.

### 타입 안전 열거형 패턴

타입 안전 열거형 패턴은 이러한 문제를 해결하기 위해 사용됩니다. 이 패턴은 각 열거형 상수를 고유한 인스턴스로 만드는 방식으로 구현됩니다.

### 예제: 타입 안전 열거형 패턴 구현

```java
public class Direction {
    public static final Direction NORTH = new Direction("NORTH");
    public static final Direction SOUTH = new Direction("SOUTH");
    public static final Direction EAST = new Direction("EAST");
    public static final Direction WEST = new Direction("WEST");

    private String name;

    private Direction(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return this.name;
    }
}
```

- **타입 안전성**: `Direction` 클래스의 열거형 상수는 `Direction` 타입이기 때문에 잘못된 타입이 전달될 가능성이 없습니다.
- **캡슐화**: `Direction` 클래스 내부에서만 열거형 상수를 정의하므로, 잘못된 값이 정의될 가능성을 제거합니다.
- **확장성**: 클래스 내부에 새로운 상수를 추가할 수 있으며, 각 상수는 독립적인 객체로 다룰 수 있습니다.

### 타입 안전 열거형 패턴의 단점

- **코드의 복잡성**: 열거형을 만들기 위해 많은 코드를 작성해야 하며, 이 때문에 코드가 다소 복잡해질 수 있습니다.
- **상수 갯수 제한**: 동일한 이름의 상수를 추가하기 어렵고, 상수 간의 비교가 `==`가 아닌 `equals()`로 이루어져야 합니다.

### Java의 `enum` 도입 이후

Java 1.5부터는 `enum` 키워드를 사용하여 열거형을 정의할 수 있게 되었으며, 이는 타입 안전 열거형 패턴의 장점과 단점을 보완한 문법입니다.

```java
public enum Direction {
    NORTH, SOUTH, EAST, WEST;
}
```

이제 `enum`을 사용하면 더 간단하고 직관적으로 타입 안전한 열거형을 만들 수 있습니다. `enum`은 내부적으로 `final` 클래스이며, 각 열거형 상수는 고유한 인스턴스입니다.

### 요약
- **타입 안전 열거형 패턴**은 Java에서 타입 안전하고 확장 가능한 열거형을 구현하기 위한 패턴입니다.
- Java 1.5 이후 도입된 `enum` 키워드는 이 패턴을 간단하고 효과적으로 구현할 수 있게 합니다.