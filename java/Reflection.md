# Java의 Reflection 기술

**Reflection**은 Java에서 동적으로 클래스, 메소드, 필드, 생성자 등에 접근하고 조작할 수 있게 해주는 강력한 기능입니다. 주로 런타임에 객체의 메타데이터(예: 클래스의 구조, 메소드, 필드 등)에 접근하고, 이를 이용해 동적으로 인스턴스를 생성하거나 메소드를 호출하는 데 사용됩니다.

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
