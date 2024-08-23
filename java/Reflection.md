# Java의 Reflection 기술

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