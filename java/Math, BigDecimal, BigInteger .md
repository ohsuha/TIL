# Math
> java.lang.Math

기본적인 수학계산에 유용한 메서드들로 구성되어 있다.

## round(), rint()
소수점 n번째 자리에서 반올림한 값을 얻기 위해서 사용한다.
항상 소수점 첫째 자리에서 반올림을 해서 정수값(long)을 결과로 돌려준다.
1. 원래 값에 100을 곱한다. : 90.7552 * 100 -> 9075.52
2. 위의 결과에 Math.round()를 사용한다. : Math.round(9075.52) -> 9076
3. 위의 결과를 다시 100.0으로 나눈다. : 9076 / 100.0 -> 90.76

rint() 도 소수점 첫 째자리에서 반올림하지만, 반환값이 double 이다.<br>
두 정수의 정가운데 있는 값은 가장 가까운 짝수 정수를 반환한다.<br>
-1.5에 대해서 round()는 -1.0 , rint()는 -2.0 으로 나온다.

### Exact
정수형간의 연산에서 발생할 수 있는 오버플로우를 감지하기 위한 메서드들이다. 오버플로우가 발생하면, 예외(ArithmeticException) 을 발생시킨다.

### sqrt()
제곱근을 계산해준다.
### pow()
n 제곱을 계산해준다.

### StrictMath 클래스
JVM은 사실 설치된 OS 의 메서드를 호출해서 OS 에 의존적인 계산을 하고 있다.<br>
부동소수점 계산이 OS 마다 달라서 컴퓨터마다 결과가 다를 수 있다.<br>
이러한 차이를 없애기 위해 성능은 다소 포기하는 대신, 어떤 OS에서도 같은 결과를 얻도록 작성된 클래스이다.

# BigInteger
> java.math.BigInteger

- 정수형으로 표현할 수 있는 값의 한계가 있다. long 보다 큰 값을 다뤄야할때 사용하면 좋은 클래스이다.
- 내부적으로 int 배열을 사용해서 값을 다룬다. 대신 성능은 long보다 떨어진다.
- Stirng 과 같은 불변이다.
- 2의 보수 형태로 표현, 부호를 int signum에 따로 저장하고 int mag[] 배열에는 값을 저장한다.
- 정수형 리터럴로는 표현할 수 있는 값의 한계가 있어 문자열 String으로 값을 표현한다.

## 변환 메소드
| 메소드                  | 설명                                                                                         |
|-------------------------|----------------------------------------------------------------------------------------------|
| `intValue()`            | `BigInteger` 값을 `int` 타입으로 변환합니다. 값이 `int`의 범위를 초과하면 하위 32비트만 반환됩니다.  |
| `longValue()`           | `BigInteger` 값을 `long` 타입으로 변환합니다. 값이 `long`의 범위를 초과하면 하위 64비트만 반환됩니다. |
| `floatValue()`          | `BigInteger` 값을 `float` 타입으로 변환합니다. 정밀도가 손실될 수 있습니다.                    |
| `doubleValue()`         | `BigInteger` 값을 `double` 타입으로 변환합니다. 정밀도가 손실될 수 있습니다.                   |
| `shortValue()`          | `BigInteger` 값을 `short` 타입으로 변환합니다. 값이 `short`의 범위를 초과하면 하위 16비트만 반환됩니다. |
| `byteValue()`           | `BigInteger` 값을 `byte` 타입으로 변환합니다. 값이 `byte`의 범위를 초과하면 하위 8비트만 반환됩니다.   |
| `toByteArray()`         | `BigInteger` 값을 2의 보수 표현의 바이트 배열로 변환합니다.                                     |
| `toString()`            | `BigInteger` 값을 10진수 문자열로 변환합니다.                                                   |
| `toString(int radix)`   | `BigInteger` 값을 지정된 진수(radix)의 문자열로 변환합니다.                                      |

## 연산 메소드
| 메소드                | 설명                                                                                        |
  |-----------------------|---------------------------------------------------------------------------------------------|
| `add(BigInteger val)`           | 두 `BigInteger` 값을 더합니다.                                                        |
| `subtract(BigInteger val)`      | 두 `BigInteger` 값을 뺍니다.                                                          |
| `multiply(BigInteger val)`      | 두 `BigInteger` 값을 곱합니다.                                                        |
| `divide(BigInteger val)`        | 두 `BigInteger` 값을 나눕니다.                                                        |
| `mod(BigInteger val)`           | 두 `BigInteger` 값을 나눈 나머지를 반환합니다.                                         |
| `pow(int exponent)`             | 현재 `BigInteger` 값을 주어진 지수로 거듭제곱합니다.                                   |
| `gcd(BigInteger val)`           | 두 `BigInteger` 값의 최대공약수를 구합니다.                                           |
| `abs()`                         | 현재 `BigInteger` 값의 절대값을 반환합니다.                                           |
| `negate()`                      | 현재 `BigInteger` 값의 부호를 반대로 바꾼 값을 반환합니다.                             |
| `equals(Object x)`              | 현재 `BigInteger` 값과 지정된 객체의 값이 같은지 비교합니다.                          |
| `compareTo(BigInteger val)`     | 현재 `BigInteger` 값과 지정된 `BigInteger` 값을 비교합니다.                             |
| `and(BigInteger val)`           | 두 `BigInteger` 값의 비트 AND 연산을 수행합니다.                                       |
| `or(BigInteger val)`            | 두 `BigInteger` 값의 비트 OR 연산을 수행합니다.                                        |
| `xor(BigInteger val)`           | 두 `BigInteger` 값의 비트 XOR 연산을 수행합니다.                                       |
| `not()`                         | 현재 `BigInteger` 값의 비트를 반전합니다.                                             |
| `shiftLeft(int n)`              | 현재 `BigInteger` 값을 주어진 n만큼 왼쪽으로 비트 시프트합니다.                        |
| `shiftRight(int n)`             | 현재 `BigInteger` 값을 주어진 n만큼 오른쪽으로 비트 시프트합니다.                      |
| `toString(int radix)`           | 현재 `BigInteger` 값을 지정된 진수(radix)의 문자열로 변환합니다.                       |
| `bitCount()`                    | 현재 `BigInteger` 값의 1로 설정된 비트 수를 반환합니다.                               |
| `bitLength()`                   | 현재 `BigInteger` 값의 최소 비트 길이를 반환합니다.                                   |
| `isProbablePrime(int certainty)`| 현재 `BigInteger` 값이 소수인지 여부를 지정된 확률로 테스트합니다.                     |
| `min(BigInteger val)`           | 두 `BigInteger` 값 중 작은 값을 반환합니다.                                           |
| `max(BigInteger val)`           | 두 `BigInteger` 값 중 큰 값을 반환합니다.                                             |


# BigDecimal
- 정수를 이용해서 실수를 표현한다. 실수를 정수와 10의 제곱의 곱으로 표현한다.
- BigDecimal은 정수를 저장하는데 BigInteger 를 사용한다.
```java
import java.math.BigInteger;
private final BigInteger intVal; //정수
private final int scale; // 지수
private transient int precision; // 정밀도 : 정수의 자리수
```
- 문자열로 숫자를 표현한다.
- double 타입의 값을 매개변수로 갖는 생성자를 사용하면 오차가 발생할 수 있다.
## 변환 메소드
| 메소드                                 | 설명                               |
|----------------------------------------|------------------------------------|
| `toString()`                           | `BigDecimal` 값을 문자열로 변환합니다.   |
| `toPlainString()`                      | 지수 표기 없이 `BigDecimal` 값을 문자열로 변환합니다. |

## 연산 메서드
| 메소드                                 | 설명                               |
|----------------------------------------|------------------------------------|
| `add(BigDecimal augend)`               | 두 `BigDecimal` 값을 더합니다.        |
| `subtract(BigDecimal subtrahend)`      | 두 `BigDecimal` 값을 뺍니다.          |
| `multiply(BigDecimal multiplicand)`    | 두 `BigDecimal` 값을 곱합니다.        |
| `divide(BigDecimal divisor)`           | 두 `BigDecimal` 값을 나눕니다.        |
| `divide(BigDecimal divisor, int scale, RoundingMode roundingMode)` | 나누기 연산 후 반올림 모드를 적용하여 결과를 반환합니다. |

RoundingMode 는 ㅂ반올림 처리 방법에 대한 것이다.

| RoundingMode             | 설명                                                                                     |
|--------------------------|------------------------------------------------------------------------------------------|
| `CEILING`                | 양수 방향으로 올림. 음수에서는 절대값이 더 작아지도록, 양수에서는 더 커지도록 반올림.          |
| `FLOOR`                  | 음수 방향으로 내림. 음수에서는 절대값이 더 커지도록, 양수에서는 더 작아지도록 반올림.          |
| `UP`                     | 0에서 멀어지는 방향으로 올림. 절대값이 더 크게 반올림됩니다.                                 |
| `DOWN`                   | 0에 가까운 방향으로 내림. 절대값이 더 작아지도록 반올림됩니다.                                 |
| `HALF_UP`                | 반올림, 반값 이상일 때 올림. 예를 들어 5.5는 6, 2.5는 3으로 반올림됩니다.                   |
| `HALF_DOWN`              | 반올림, 반값 이상일 때 내림. 예를 들어 5.5는 5, 2.5는 2로 반올림됩니다.                   |
| `HALF_EVEN`              | 반올림, 반값에서는 가장 가까운 짝수로 반올림. "은행가 반올림"이라고도 불림. 예: 5.5는 6, 4.5는 4.  |
| `UNNECESSARY`            | 반올림 없이 정확히 표현 가능해야 하며, 그렇지 않으면 예외가 발생합니다.                         |
