# java.lang
JAVA 프로그래밍에 필요한 가장 기본적인 클래스들이 모여있는 패키지이다. <br>
별도의 import 없이 사용할 수 있다.

# Object.class
[Object.class](https://github.com/ohsuha/TIL/blob/master/java/Object.class.md)
>모든 클래스의 최상위 부모 클래스, java.lang 에 선언되어있다.

Object 클래스에 있는 메소드들을 통해 클래스의 기본적인 행동을 정의할 수 있기 때문이다. 클래스라면 이 정도의 메소드는 정의되어 있어야 하고, 처리해 주어야 한다는 것을
정의하는 작업이 필요하기 때문에 Object 클래스를 상속 받았다고 생각하면 된다.

## annotation
1. @Deprecated : 더 이상 사용하지 않음을 나타냄
2. @SuppressWarning : 컴파일시 경고를 무시하겠다는 뜻
3. @Override : 오버라이딩된 메소드라는 뜻

## exception
1. OOM(out of memory) : 자바의 메모리가 부족하면 발생하는 에러
2. StackOverFlow : 호출된 관계가 너무 많아서, 재귀 메소드를 잘못 작성했다면 스택(어떤 메소드가 어떤 메소드를 호출했는지)에 쌓을 수 있는 메소드의 호출 정보에 한계를 넘을 수 있다. 이 경우 발생하는 에러

## wrapper class
자바의 기본 자료형은 stack 에 저장된다. 이러한 기본 자료형을 참조자료형, 객체로 처리해야 할 때가 있다.<br>
기본 자료형으로 선언된 타입의 참조 자료형 클래스를 wrapper 클래스라고 한다.<br>
모두 Number 클래스를 확장한다.<br>
자바 1.5부터는 자바 컴파일러가 자동으로 형변환을 해줘서 기본형 처럼 사용할 수 있다. (AutoBoxing 과 오토 언박싱AutoUnBoxing) 
1. 매개 변수로 참조 변수가 요구 될 때.
2. 제네릭에서 사용하기 위해
3. MIN_VALUE, MAX_VALUE 와 같은 상수를 사용하기 위해.
4. 2, 8, 10, 16 진수 변환을 쉽게 처리하기 위해 (toBinarayString(), toHexString())

## System
시스템에 대한 정보를 확인하는 클래스, JVM 종료와 GC 관련 메소드는 사용하지 않는 것을 권장한다.

## Math
Math 클래스는 수학에서 자주 사용하는 상수들과 함수들을 미리 구현해 놓은 클래스<br>
Math 클래스의 모든 메소드는 클래스 메소드(static method)이므로, 객체를 생성하지 않고도 바로 사용할 수 있다.

## String class
[String.class](https://github.com/ohsuha/TIL/blob/master/java/String.class.md)<br>
불변 클래스(immutable class) : String 인스턴스는 한 번 생성되면 그 값을 읽기만 할 수 있고, 변경할 수는 없다.

## StringBuffer, StringBuilder
String 클래스의 인스턴스는 immutable object이지만, StringBuffer 클래스의 인스턴스는 그 값을 변경할 수도 있고, 추가할 수도 있다.<br>
이를 위해 StringBuffer 클래스는 내부적으로 버퍼(buffer)라고 하는 독립적인 공간을 가진다. 버퍼 크기의 기본값은 16개의 문자를 저장할 수 있는 크기이며, 생성자를 통해 그 크기를 별도로 설정할 수도 있다.<br>
StringBuffer 인스턴스를 사용하면 문자열을 바로 추가할 수 있으므로,  공간의 낭비도 없으며 속도도 매우 빨라집니다. (String 인스턴스는 새로운 객체를 생성하므로 공간이 낭비되고, 속도도 느려진다.)

