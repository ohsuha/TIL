# String.class

```java
public final class String implements java.io.Serializable, Comparable<String>,
                                     CharSequence, Constable, ConstantDesc {}
```
- final 클래스로 상속이 불가능한 클래스이다.
  - 불변 객체는 생성된 후에 그 상태를 변경할 수 없는 객체로, 코드의 안정성과 예측 가능성을 높이는 데 매우 유용합니다. 특히, 멀티스레드 환경에서 안전하며, 재사용 및 캐싱이 용이합니다.
  - 그러나 상태 변경 시 새로운 객체를 생성해야 하기 때문에 성능에 영향을 미칠 수 있으며, 메모리 사용량이 증가할 수 있다는 단점도 있습니다.
- 문자열을 저장하기 위해 문자형 배열 참조변수 char[] value 를 인스턴스 변수로 정의해 놨다.
- 한글은 EUC-KR (4바이트), UTF-16 (6바이트) 를 사용하고 스트링에 캐릭터 셋을 지정해 바이트-> 스트링으로 바꿀때 영어외의 언어도 처리할 수 있다.
- String은 불변 객체이기 때문에 값을 수정할 수 없다. 수정시 기존 객체는 버려지고 새로운 주소 값에 할당된 값을 가져온다.
- 따라서 스트링을 더하는 작업이 많을 떄에는 `StringBuilder`나 `StringBuffer`를 사용한다.
## 자바 문자열 풀(Java String Constant Pool)
- 힙 메모리의 특수한 영역으로 동일한 문자열을 재사용하기 위해 관리하는 메모리 공간
- 메모리 효율을 향상시키기 위한 목적으로 사용된다.
-  동일한 문자열 리터럴을 재사용하는 메커니즘입니다. 리터럴로 생성된 문자열은 풀에 저장되어, 동일한 문자열이 여러 번 사용될 때마다 새 객체를 생성하는 대신 풀에서 기존 객체를 재사용합니다. 이는 메모리를 절약하고 성능을 향상시키는 중요한 역할을 합니다.
- ```commandline
    String str1 = "Hello";
    String str2 = "Hello";
    ```
    - 이 경우 Java는 문자열 풀에서 "Hello"라는 문자열이 이미 존재하는지 확인하고, 존재한다면 그 주소를 str1과 str2에 할당
    - str1 == str2 가된다.
    - 리터럴 문자열은 풀에 저장: Java는 리터럴로 선언된 문자열을 문자열 풀에 저장합니다. 동일한 리터럴이 여러 번 사용될 경우, 새로운 메모리를 할당하지 않고 이미 풀에 존재하는 문자열의 참조를 반환합니다.
- ```commandline
    String str3 = new String("Hello");
    ```
    - 이 경우 새로운 String 객체를 힙 메모리에 생성한다.
    - new 를 통해 생성하므로 str1 == str3 은 false 가 된다.
    - 하지만 str3.equals(str1) 은 true 인데 이것은 String 에서 오버라이딩한 equals 가 문자열의 값을 비교하기 때문
    - 문자열 풀에 있으면 문자열 비교 시 참조 비교(==)를 사용할 수 있어 성능이 향상될 수 있습니다. 이는 값 비교(equals())에 비해 훨씬 빠릅니다.

## 주요 메서드
| 메서드                            | 설명                                   | 예제                                | 결과                          |
|-----------------------------------|----------------------------------------|-------------------------------------|-------------------------------|
| `charAt(int index)`               | 주어진 인덱스에 있는 문자를 반환함       | `"hello".charAt(1)`                 | `'e'`                         |
| `concat(String str)`              | 문자열을 이어 붙여 새로운 문자열을 반환함| `"hello".concat(" world")`          | `"hello world"`               |
| `contains(CharSequence s)`        | 문자열에 특정 문자가 포함되어 있는지 확인함 | `"hello".contains("ell")`           | `true`                        |
| `equals(Object anObject)`         | 두 문자열이 동일한지 비교함              | `"hello".equals("hello")`           | `true`                        |
| `equalsIgnoreCase(String another)`| 대소문자를 무시하고 두 문자열이 동일한지 비교함 | `"Hello".equalsIgnoreCase("hello")` | `true`                        |
| `indexOf(int ch)`                 | 특정 문자의 첫 번째 인덱스를 반환함      | `"hello".indexOf('l')`              | `2`                           |
| `indexOf(String str)`             | 특정 문자열의 첫 번째 인덱스를 반환함    | `"hello".indexOf("lo")`             | `3`                           |
| `isEmpty()`                       | 문자열이 비어있는지 확인함              | `"".isEmpty()`                      | `true`                        |
| `length()`                        | 문자열의 길이를 반환함                  | `"hello".length()`                  | `5`                           |
| `replace(char oldChar, char newChar)` | 문자열 내의 특정 문자를 다른 문자로 대체함 | `"hello".replace('l', 'p')`         | `"heppo"`                     |
| `replace(CharSequence target, CharSequence replacement)` | 문자열 내의 특정 문자열을 다른 문자열로 대체함 | `"hello world".replace("world", "Java")` | `"hello Java"` |
| `substring(int beginIndex)`       | 주어진 인덱스부터 문자열의 끝까지 부분 문자열을 반환함 | `"hello".substring(2)`              | `"llo"`                       |
| `toLowerCase()`                   | 문자열을 소문자로 변환함               | `"HELLO".toLowerCase()`             | `"hello"`                     |
| `toUpperCase()`                   | 문자열을 대문자로 변환함               | `"hello".toUpperCase()`             | `"HELLO"`                     |
| `trim()`                          | 문자열 양쪽 끝의 공백을 제거함          | `"  hello  ".trim()`                | `"hello"`                     |

### join()
여러 문자열 사이에 구분자를 넣어서 결합한다.
```java
String animals = "dog,cat,bear";
String[] arr = animals.split(",");
String str = String.join("-", arr);

StringJoiner stringJoiner = new StringJoiner(",", "[", "]");
String[] strArr = {"aaa", "bbb", "ccc"};
for(String s : strArr){
    sj.add(s.toUpperCase()); // [AAA,BBB,CCC]
}
```
### getBytes(String charsetName)
문자열의 문자 인코딩을 다른 인코딩으로 변경할 수 있다.
```java
byte[] utf8_str = "가".getBytes("UTF-8"); // 문자열을 UTF-8로 변환
String str = new String(utf8_str, "UTF-8"); // byte 배열을 문자열로 변환
```

### String.format()
형식화된 문자열을 만들어내는 메서드
```java
String str = Stirng.format("%d 더하기 %d 는 %d", 3, 5, 3+5);
```

### String.valueOf(기본형), valueOf(String s), parseInt(String s)
기본 숫자형을 문자로, 스트링을 기본 숫자 형으로 변환해주는 메서드들이다.

## 구현 인터페이스
<B>Serializable</B> : 해당 객체를 파일로 저장하거나 다른 서버에 전송 가능한 상태가 된다. <br>
<B>Comparable<String></B> : equals 와 같이 값을 비교하는 것인데 리턴타입이 `int`로 같으면 0, 다른데 보다 앞에 있으면 -1, 
뒤에 있으면 1을 리턴한다. 객체의 순서 처리에 사용된다.<br>
<B>CharSequence<String></B> : 해당 클래스가 문자열을 다룬다는 것을 명시적으로 나타내기 위해 사용된다.  <br>

## StringBuilder 와 StringBuffer
- 내부적으로 문자열 편집을 위한 버퍼를 가지고 있다.
  - char형 배열의 참조변수를 인스턴스 변수로 선언해 놓고 있다.
  - 이때 생성한 char 형 배열을 인스턴스변수 value가 참조하게 된다.
  - 인스턴스에 저장될 문자열의 길이를 고려하여 여유있는 크기를 지정해주는 것이좋다. 버퍼의 길이를 늘려주는 작업이 추가되면 작업효율이 떨어진다.(기본 16자리)
- StringBuilder는 스레드의 동시성 이슈에서 안전하지 않다. 하지만 StringBuffer 보다 빠르다. StringBuffer는 스레드에 안전하지만 느리다.
- 둘 다 append() 메소드를 통해 객체를 생성해 더한다.
- jdk5 에서는 String 에 + 연산시 자동으로 StringBuilder 를 사용하도록 변환해주지만 for 루프 안에서는 자동변환이 되지 않으니 꼭 따로 사용해 주어야 한다.
- equals 메소드가 구현되어있지 않아서 ==과 동일한 결과가 나온다. toString() 으로 변환하여 비교해주자
  
 | 메서드                             | 설명                                     | 예제                                            | 결과                          |
  |------------------------------------|------------------------------------------|-------------------------------------------------|-------------------------------|
  | `append(String str)`               | 문자열을 버퍼의 끝에 추가함               | `new StringBuffer("Hello").append(" World")`     | `"Hello World"`               |
  | `insert(int offset, String str)`   | 지정된 위치에 문자열을 삽입함             | `new StringBuffer("Hello").insert(5, " World")`  | `"Hello World"`               |
  | `replace(int start, int end, String str)` | 지정된 범위의 문자열을 새 문자열로 대체함 | `new StringBuffer("Hello").replace(0, 2, "Hi")`  | `"Hillo"`                     |
  | `delete(int start, int end)`       | 지정된 범위의 문자열을 삭제함             | `new StringBuffer("Hello").delete(0, 2)`         | `"llo"`                       |
  | `deleteCharAt(int index)`          | 지정된 인덱스의 문자를 삭제함             | `new StringBuffer("Hello").deleteCharAt(1)`      | `"Hllo"`                      |
  | `reverse()`                        | 문자열을 역순으로 변경함                  | `new StringBuffer("Hello").reverse()`            | `"olleH"`                     |
  | `charAt(int index)`                | 주어진 인덱스에 있는 문자를 반환함        | `new StringBuffer("Hello").charAt(1)`            | `'e'`                         |
  | `setCharAt(int index, char ch)`    | 주어진 인덱스의 문자를 변경함             | `new StringBuffer("Hello").setCharAt(1, 'a')`    | `"Hallo"`                     |
  | `length()`                         | 문자열의 길이를 반환함                    | `new StringBuffer("Hello").length()`             | `5`                           |
  | `substring(int start, int end)`    | 지정된 범위의 부분 문자열을 반환함        | `new StringBuffer("Hello").substring(1, 4)`      | `"ell"`                       |
  | `capacity()`                       | 현재 버퍼의 용량을 반환함                 | `new StringBuffer("Hello").capacity()`           | `21` (초기 값에 따라 다름)   |
  | `ensureCapacity(int minimumCapacity)` | 최소한의 버퍼 용량을 보장함              | `new StringBuffer().ensureCapacity(50)`          | (버퍼 용량이 50 이상으로 증가)|
  | `trimToSize()`                     | 사용하지 않는 용량을 제거하고 실제 길이에 맞게 버퍼 크기를 조정함 | `new StringBuffer().trimToSize()`                | (버퍼 용량이 문자열의 길이에 맞게 감소) |
