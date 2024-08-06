# String.class

```commandline
public final class String implements java.io.Serializable, Comparable<String>,
                                     CharSequence, Constable, ConstantDesc {}
```
- final 클래스로 상속이 불가능한 클래스이다.
- 한글은 EUC-KR (4바이트), UTF-16 (6바이트) 를 사용하고 스트링에 캐릭터 셋을 지정해 바이트-> 스트링으로 바꿀떄 영어외의 언어도 처리할 수 있다.
- String은 불변 객체이기 때문에 값을 수정할 수 없다. 수정시 기존 객체는 버려지고 새로운 주소 값에 할당된 값을 가져온다.
- 따라서 스트링을 더하는 작업이 많을 떄에는 `StringBuilder`나 `StringBuffer`를 사용한다.

## 구현 인터페이스
<B>Serializable</B> : 해당 객체를 파일로 저장하거나 다른 서버에 전송 가능한 상태가 된다. <br>
<B>Comparable<String></B> : equals 와 같이 값을 비교하는 것인데 리턴타입이 `int`로 같으면 0, 다른데 보다 앞에 있으면 -1, 
뒤에 있으면 1을 리턴한다. 객체의 순서 처리에 사용된다.<br>
<B>CharSequence<String></B> : 해당 클래스가 문자열을 다룬다는 것을 명시적으로 나타내기 위해 사용된다.  <br>

## StringBuilder 와 StringBuffer
- StringBuilder는 스레드의 동시성 이슈에서 안전하지 않다. 하지만 StringBuffer 보다 빠르다. StringBuffer는 스레드에 안전하지만 느리다.
- 둘 다 append() 메소드를 통해 객체를 생성해 더한다.
- jdk5 에서는 String 에 + 연산시 자동으로 StringBuilder 를 사용하도록 변환해주지만 for 루프 안에서는 자동변환이 되지 않으니 꼭 따로 사용해 주어야 한다.
