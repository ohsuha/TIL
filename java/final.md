# final

## final class
```
public final class ClassName {}
```
상속이 불가능해진다. 누군가 이 클래스를 확장해선 안될때 사용한다. 대표적으로 String 클래스가 있다.

## method
```
public final void MethodName() {}
```
overriding이 불가능해진다.

## variable
```java
private fianl Stirng EVENT_TOPIG = "event.topic";
public final int = 123;
```
더이상 값을 바꿀 수 없는 변수를 의미하게 된다.
멤버 변수, 지역 변수에 사용되면 값을 변경할 수 없는 상수가 된다.
선언과 함께 초기화 하거나 생성자를 통해 초기화 해줘야한다.
객체가 final 이라도 객체 안의 멤버들이 final이 아니라면 변경이 가능하다.