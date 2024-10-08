# Java의 Reflection 기술
리플렉션은 클래스가 제공하는 다양한 정보들을 동적으로 분석하고 사용하는 기능이다.
프로그램 실행 중에 클래스, 메서드, 필드의 정보를 얻거나 새로운 객체를 생성, 메소드 호출이 가능하다.

- private 접근 제어자에도 접근해서 값을 변경할 수 있다.
- 성능이 느려진다.
- 컴파일 에러가 뜨지 않는다.

이런 단점들에도 불구하고 사용하는데 왜 쓰는걸까?

1. 일반적인 비즈니스 로직에는 사용하지 않고 라이브러리나 테스트코드에서 사용한다.
2. 리플렉션을 활용해 어떤 객체를 받아서 필드값을 조회한 다음 Null 이면 기본 값을 설정해 주는 식으로,
   여러 객체에 공통적으로 기본값을 넣어주는 등의 기존 코드로 해결하기 어려운 공통 문제를 손쉽게 처리할 수 있게 도와준다.
   객체를 직렬화하거나 역직렬화 하는것도 이런 예시가 될 수 있다. 객체의 구조를 미리 알지 않아도 런타임에 동적으로 필드와 메서드에 접근할 수 있고 프라이빗값도 접근할 수 있어 자유롭게 직렬화가 가능하다.
3. Transactional 애너테이션은 placeOrder 메서드가 트랜잭션 범위 내에서 실행되도록 만듭니다. Spring은 리플렉션을 사용해 OrderService 클래스의 메서드에 지정된 애너테이션을 스캔하고, 트랜잭션 설정을 동적으로 적용합니다.
4. 이름만 가지고 객체 생성, 메소드 실행이 가능하므로 서블릿에서는 URL 요청 경로를 가지고 경로와 메소드이름이 맞는지 체크해서 invoke 를 통해 동적으로 호출할 수 있다.
5. 스프링에서는 service 에 repository 같은 것을 의존할때 리플렉션을 사용해 repository 객체를 service 의 repository 필드에 주입할 수 있다.

즉 컴파일 시점에는 알 수 없는 클래스, 메서드, 필드에 동적으로 접근할 수 있다.
직렬화,역직렬화 같은 여러 객체의 공통문제를 손쉽게 처리할 수 있다.
프레임워크에서 리플렉션을 활용하여 개발자에 편리한 기능을 제공할 수 있다.(의존성 주입, AOP, 어노테이션)

> test code : https://github.com/ohsuha/code_scratch/blob/master/code_scratch/src/test/java/ReflectionTest.java

**Reflection**은 Java에서 런타임 시점에서 클래스의 메타데이터를 분석하고, 객체의 속성이나 메소드를 동적으로 접근할 수 있는 기능을 제공

#### 주요 기능:
1. **클래스 정보 획득**: `Class<?>` 객체를 사용해 클래스의 이름, 부모 클래스, 구현된 인터페이스 등을 조회할 수 있습니다.
   ```java
   Class<?> clazz = Class.forName("java.util.ArrayList");
   ```

2. **필드 접근**: 클래스의 필드에 접근하여 값을 읽거나 쓸 수 있습니다.
   ```java
   Field field = clazz.getDeclaredField("size");
   field.setAccessible(true); // private 필드에 접근 가능하게 설정
   int size = (int) field.get(arrayListInstance);
   ```

3. **메소드 호출**: 메소드 객체를 통해 메소드를 호출할 수 있습니다.
   ```java
   Method method = clazz.getDeclaredMethod("add", Object.class);
   method.setAccessible(true);
   method.invoke(arrayListInstance, "Hello");
   ```

4. **생성자 호출**: 생성자를 통해 객체를 동적으로 생성할 수 있습니다.
   ```java
   Constructor<?> constructor = clazz.getConstructor();
   Object instance = constructor.newInstance();
   ```

5. **배열 조작**: 배열의 길이를 조회하거나 배열 요소에 접근하는 등 배열과 관련된 작업도 Reflection을 통해 수행할 수 있습니다.
   ```java
   int length = Array.getLength(arrayInstance);
   ```

#### Reflection의 사용 사례:
- **프레임워크**: Spring이나 Hibernate와 같은 프레임워크에서 빈(bean) 객체의 생성 및 초기화, 의존성 주입 등을 수행할 때 Reflection을 자주 사용합니다.
- **라이브러리**: JSON 파싱 라이브러리(Gson, Jackson)에서 객체와 JSON 간의 변환 작업을 할 때, 객체의 필드에 접근하기 위해 Reflection을 사용합니다.
- **테스트**: 단위 테스트에서 비공개(private) 메소드를 테스트하기 위해 Reflection을 사용합니다.

#### 단점:
- **성능 저하**: Reflection을 사용하면 일반적인 메소드 호출보다 성능이 떨어질 수 있습니다.
- **컴파일 타임 안전성 부족**: Reflection을 사용하면 컴파일 시점에서 오류를 잡기 어렵고, 런타임에서 예외가 발생할 가능성이 있습니다.
- **보안 문제**: 잘못 사용하면 보안 문제가 발생할 수 있습니다. (예: `setAccessible(true)`로 private 필드에 접근)

#### declare 차이
- declare 가 붙으면
  - 해당 클래스에 선언된 모든 메소드 필드,생성자를 반환
  - 접근 제어자에 상관없이 모두 반환한다
  - 상속받은 멤버는 포함하지 않는다
- 없으면
  - 해당 클래스와 상위 클래스에서 public 메소드, 필드,생성자를 반환
  - 상속받은 public 메소드도 포함