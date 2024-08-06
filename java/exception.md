# exception

![img](https://www.tcpschool.com/lectures/img_java_exception_class_hierarchy.png)
### checked exception
아래 두개를 제외한 모든 예외, 하지만 Exception 으로 만들지 말자
### error
자바 프로그램 밖에서 발생한 예외, 프로세스에 영향을 준다.
### runtime exception
예외가 발생할 것을 미리 예상치 못했을때 발생한다. 컴파일시에는 발생하지 않지만 실행시 발생한다. 컴파일때 체크하지 않는다. 스레드에 영향을 준다.

> 예외를 만들때에는 Throwable 의 직계 자손 클래스를 상속해 만들어야 한다.즉, 사용자가 직접 구현하는 unchecked throwable은 모두 RuntimeException의 하위 클래스여야 한다.
 


## 자바 예외 전략
- 어떠한 예외도 무시하지 말라
- 적절한 수준에서의 예외처리, 발생한 예외를 가장 잘 이해하고 처리할 수 있는 부분에서 예외를 캐치해 처리해야 한다.
- 구체적인 예외를 캐치하자, 예외상황을 명확히 하자

## 예외 처리 패턴
- 저수준 예외를 캐치하여, 그것을 더 구체적이거나 의미 있는 예외로 변환해 다시 던지자
- 가능한 한 체크 예외의 사용을 최소화하고, 대신 언체크 예외를 사용
  - 특정 메소드에서 checked exception을 throw하고 상위 메소드에서 그 exception을 catch 한다면, 모든 중간단계 메소드에 exception을 throws해야한다.
  - 상위 메소드에서 하위 레벨 메소드의 디테일에 대해 알아야하기 때문에 OCP 원칙 위배이다.
- 예외 로깅 전략을 사용해 오류의 원인을 명확히 가고 추적을 용이하게 해야한다.