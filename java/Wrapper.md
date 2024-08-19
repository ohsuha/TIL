# wrapper 클래스
성능을 위해 8개의 기본형은 객체로 다루지 않는다. 하지만 때로는 기본형도 객체로 다뤄야하는 경우가 있다.
- 매개변수로 객체를 요구할 때
- 기본형 값이 아닌 객체로 저장해야할 때
- 객체간의 비교가 필요할 때 등등..

8개의 기본형을 대표하는 8개의 래퍼클래스가 있고 이를 이용하면 기본형 값을 객체로 다룰 수 있다.
<br>
모두 equals() 메소드를 오버라이딩 하고 있어서 주소값이 아닌 객체가 가지고 있는 값을 비교한다.
<br>
비교연산자 <, > 는 사용할 수 없다. 대신 `compareTo()` 메서드를 통해 비교할 수 있다.
<br>
toString() 도 오버라이딩 되어있어서 객체가 가지고 있는 값을 문자열로 변환하여 반환한다.

## method

| 메소드                           | 설명                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------|
| `parseInt(String s)`             | 문자열을 `int`로 변환합니다.                                                               |
| `valueOf(String s)`              | 문자열을 `Integer` 객체로 변환합니다.                                                      |
| `intValue()`                     | `Integer` 객체를 `int` 타입으로 변환합니다.                                                 |
| `compareTo(Integer anotherInt)`  | 두 `Integer` 객체를 비교합니다.                                                             |
| `toString()`                     | `Integer` 객체를 문자열로 변환합니다.                                                       |
| `hashCode()`                     | `Integer` 객체의 해시 코드를 반환합니다.                                                    |

## Number 클래스
추상 클래스로 내부적으로 숫자를 멤버변수로 갖는 래퍼클래스들의 조상이다.<br>
넘버 클래스는 객체가 가지고 있는 값을 숫자와 관련된 기본형으로 반환하여 반환하는 메서드들을 정의하고 있다

```java
java.lang.Number

public abstract class Number implements java.io.Serializable {
    public Number() {super();}

    public abstract int intValue();

    public abstract long longValue();

    public abstract float floatValue();

    public abstract double doubleValue();

    public byte byteValue() {
        return (byte)intValue();
    }
    public short shortValue() {
        return (short)intValue();
    }

    @java.io.Serial
    private static final long serialVersionUID = -8742448824652078965L;
    
}
```

```java
java.lang.Integer

public final class Integer extends Number
        implements Comparable<Integer>, Constable, ConstantDesc {
    private final int value;
    public static final Class<Integer>  TYPE = (Class<Integer>) Class.getPrimitiveClass("int");
    
    @IntrinsicCandidate
    public int intValue() {
        return value;
    }

    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }}

    public boolean equals(Object obj) {
        if (obj instanceof Integer) {
            return value == ((Integer)obj).intValue();
        }
        return false;
    }
```

## 오토박싱 언박싱
JDK1.5 이전에서는 래퍼클래스로 기본형을 객체로 만들어서 연산해야했다.<bR>
그러나 이제는 기본형과 참조형간의 덧셈이 가능하다. 컴파일러가 자동으로 변환하는 코드를 넣어주기 때문이다.<br>
컴파일러가 int 타입으로 변환해주는 intValue() 와 같은 메소드를 추가해준다.<br>
이처럼 기본형 값을 래퍼클래스의 객체로 자동으로 변환해주는것을 언박싱, 기본형 값을 래퍼클래스의 객체로 자동 변환해주는 것을 오토박싱이라고 한다.