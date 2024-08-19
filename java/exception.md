# exception
## 예외 클래스의 계층 구조
![img](https://www.tcpschool.com/lectures/img_java_exception_class_hierarchy.png)
### error
- 자바 프로그램 밖에서 발생한 예외, 프로세스에 영향을 준다.
- 메모리가 부족하거나 할때 발생한다. stackOverFlow

### checked exception
- Exception 클래스를 직접 상속받은 예외들 중에서 RuntimeException을 상속받지 않은 것들
- 컴파일 시점에 반드시 처리(try-catch로 잡거나 throws로 선언)해야 하는 예외
- IOException, SQLException, ClassNotFoundException 등
- 프로그램 실행 중에 발생할 수 있는 예외 상황에 대해 사전에 대비
- 컴파일러는 이러한 예외가 발생할 가능성이 있는 코드에 대해 예외 처리가 되어 있는지 검사
- 예외 처리가 되어 있지 않으면 컴파일 에러가 발생. throws 나 try-catch 로 잡아줘야한다.

### unchecked Exception
- 컴파일러가 예외 처리를 강제하지 않는 예외, 컴파일러가 예외처리를 확인하지 않는 RuntimeException 클래스들을 말한다.
- 대부분의 경우 런타임 시에 발생하며, 처리하지 않아도 컴파일 시에 에러가 발생하지 않는다.
- 프로그래머의 실수나 논리적 오류에 의해 발생한다. 따라서 코드의 결함을 수정하는 것이 주된 해결 방법
- 필요에 따라 try-catch로 처리할 수 있지만, 대부분의 경우 예외 발생 자체를 방지하는 것이 더 중요

### runtime exception
- RuntimeException은 Unchecked Exception의 대표적인 클래스
- 예외가 발생할 것을 미리 예상치 못했을때 발생한다.
- 컴파일시에는 발생하지 않지만 실행시 발생한다. 컴파일때 체크하지 않는다.
- 스레드에 영향을 준다.
- 프로그래머의 실수에 의해서 발생될 수 있는 예외들
  - 배열의 범위를 벗어난다던가, 값이 null 인 참조변수의 멤버를 호출하려 했다던가, 클래스간의 형변환 실수, 정수를 0으로 나누려고 했을때...

> 예외를 만들때에는 Throwable 의 직계 자손 클래스를 상속해 만들어야 한다.즉, 사용자가 직접 구현하는 unchecked throwable은 모두 RuntimeException의 하위 클래스여야 한다.

## 예외처리하기
예외 처리란 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것. 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것이 목적이다.
### try-catch문
```java
try{
    System.out.println(0/0);
    throw new Exception("고의로 발생시켰음"); // 생성자에 String이 들어가면 메시지로 얻을 수 있다.
} catch (ArithmeticException | NullPointerException | Exception e) { // 멀티 catch 
    e.printStackTrace();
    System.out.println("예외 메시지" + e.getMessage());
} finally {
    System.out.println("어떻게든 마지막에 실행됩니다.");
}
```
- try 블럭 내에서 예외가 발생한 경우
  - 예외가 발생하면 발생한 예외에 해당하는 클래스의 인스턴스가 만들어진다.
  - 발생한 예외와 일치하는 catch 블럭이 있는지 instanceOf() 로 확인한다.
  - 일치하는 catch 블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch 문을 빠져나가서 그 다음 문장을 계속해서 수행한다.
  - 만일 일치하는 catch 블럭이 없다면 예외는 처리되지 못한다.
- 예외가 발생하지 않은 경우에는 catch 블럭을 거치지 않고 try-catch 문을 빠져나가서 수행을 계속한다.
- printStackTrace() : 예외발생 당시의 호출스택에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
- getMessage() : 발생한 예외 클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

## 메서드에 예외 선언
```java
void method() throws InterruptedException {}
```
- 메서드를 작성할 때 메서드 내에서 발생할 가능성이 있는 예외를 메서드 선언부에 명시하여 이 메서드를 사용하는 쪽에서 이에 대한 처리를 하도록 강요할 수 있다.
- 예외를 전달받은 메서드가 또다시 자신을 호출한 메서드에게 전달할 수 있으며, 계속 호출 스택을 따라 전달되다가 main 메서드에서도 예외 처리가 되지 않으면 main 메서드가 종료되며 프로그램 전체가 종료된다.

## finally 블럭
- 예외의 발생여부에 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용된다.
- try-catch 문의 끝에 선택적으로 덧붙여 사용할 수 있으며, try-catch-finally 의 순서로 구성된다.
- try->catch->finally , try->finally 의 순으로 실행된다


## 자동 자원 반환 try-with-resources
- jdk 1.7부터 입출력에 사용되는 클래스 중 꼭 닫아줘야하는 것들에 try의 () 안에 객체를 생성하면 따로 close() 메소드를 호출하지 않아도 try 문을 벗어나는 순간 자동으로 close()가 호출된다.
- 클래스가 AutoCloseable 이라는 인터페이스를 구현한 것이어야 한다.


## 사용자 정의 예외 만들기
- 기존의 예외 클래스는 주로 Exception 을 상속받아서 checked 예외로 작성하는 경우가 많았지만
- 요즘은 예외처리를 선택적으로 할 수 있도록 RuntimeException 을 상속받아서 작성하는 쪽으로 바뀌고 있다.
- checked 예외는 반드시 예외 처리를 해주어야 하기 때문에 예외처리가 불필요한 경우에도 try-catch 문을 넣어서 코드가 복잡해지기 때문이다.
- 프로그래밍 환경이 달라진 만큼 필수적으로 처리해야만 할 것 같았던 예외들이 선택적으로 처리해도 되는 상황으로 바뀌는 경우가 발생하고 있다.

## 예외 되던지기
- 단 하나의 예외에 대해서도 예외가 발생한 메서드와 호출한 메서드, 양쪽에서 처리하도록 할 수 있다. 
- 하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을때 사용된다.
```java
void method1() throws Exception {
    try{
        throw new Exception();
    } catch (Exception e) {
        //예외에 대한 처리
        throw e;
    }
}
```
- 반환값이 있는 return 문의 경우 catch 블럭에도 return 문이 있어야한다. 예외가 발생했을 때도 값을 반환해야하기 때문이다.
```java
int method1() throws Exception {
  try{
    return 0;
  } catch (Exception e) {
    //예외에 대한 처리
    return 1;
  }
}
```
- 또는 catch 블럭에서 예외 되던지기를 해서 호출한 메서드로 예외를 전달하면, return 문이 없어도 된다.
```java
int method1() throws Exception {
  try{
    return 0;
  } catch (Exception e) {
    //예외에 대한 처리
    throw new Exception();
  }
}
```
- finally 블럭에서도 return 문을 사용할 수 있다. 이렇게 되면 최종적으로 finally 의 return 이 수행된다.

## 연결된 예외
- 한 예외가 다른 에외를 발생시킬 수 있다. 이를 원인 예외라고 한다.
- Throwable 클래스에 정의되어있는 `initCause()`에 정의되어있고 이 메서드를 통해서 원인 예외로 등록할 수 있다.
- 여러 예외를 하나의 큰 분류에 예외로 묶어서 다루기 위해서 사용된다. 두개의 예외는 서로 상속관계가 아니어도 상관없다.
- checked 예외를 unchecked 에외로 바꿀 수 있도록 하기 위해 사용된다.
```java
void startInstall() throws SpaceException {
    if(!enoughSpace())
        throw new SpaceExcpetion("설치 공간이 부족합니다.");
    
    if(!enoughMemory())
//        throw new MemoryException("메모리가 부족합니다.");
        throw new RuntimeExcpetion(new MemoryExcpetion("메모리가 부족합니다."));
}
```
- checked 예외가 발생해도 예외를 처리할 수 없는 사오항이 발생하기 시작하는데 이럴때 checekd 예외를 unchecked 예외로 바꾸면 예외처리가 선택적이 되므로 억지로 예외처리를 하지 않아도 된다.
- `initCause()` 대신 `RuntimeException(Throwable cause)` 생성자를 사용해도 된다.

## 자바 예외 전략
- 어떠한 예외도 무시하지 말라
- 적절한 수준에서의 예외처리, 발생한 예외를 가장 잘 이해하고 처리할 수 있는 부분에서 예외를 캐치해 처리해야 한다.
- 구체적인 예외를 캐치하자, 예외 상황을 명확히 하자

## 예외 처리 패턴
- 저수준 예외를 캐치하여, 그것을 더 구체적이거나 의미 있는 예외로 변환해 다시 던지자
- 가능한 한 체크 예외의 사용을 최소화하고, 대신 언체크 예외를 사용
  - 특정 메소드에서 checked exception을 throw하고 상위 메소드에서 그 exception을 catch 한다면, 모든 중간단계 메소드에 exception을 throws해야한다.
  - 상위 메소드에서 하위 레벨 메소드의 디테일에 대해 알아야하기 때문에 OCP 원칙 위배이다.
- 예외 로깅 전략을 사용해 오류의 원인을 명확히 가고 추적을 용이하게 해야한다.