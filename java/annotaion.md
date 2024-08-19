# annotaion
- 컴파일러에게 정보를 알려주거나 실행시 별도의 처리가 필요할 때 사용한다.
- 클래스, 변수, 메소드 등에 모두 사용 가능하다.
- 상속할 수 없다.
- 모든 애너테이션의 조상은 `java.lang.annotation.Annotation` 이다. 하지만 어노테이션은 상속이 허용되지 않으므로 명시적으로도 지정할 수 없다.
- `Annotation` 은 인터페이스이다.

## 표준 어노테이션
### @Override
- 부모로부터 오버라이딩한 메소드임을 나타내기 위해 사용한다.
- 오버라이딩 할 때 메서드의 이름을 잘못 적는 경우, 이 어노테이션을 붙이면 컴파일러가 에러메시지를 출력한다.

### @Deprecated
- 하위 호환성을 위해 코드를 지우지는 못했지만 더이상 사용하지 않음을 나타내기 위해 사용한다. 
- 이 애너테이션이 붙은 대상을 사용하면 컴파일 할때에 `-Xlint:deprecation` 경고 메시지가 발생한다.

### @FuntionalInterface
- 함수형 인터페이스를 올바르게 선언했는지 체크해준다.
- 함수형 인터페이스는 추상메서드가 하나뿐이어야 한다.

### @SuppressWarnings
- 빌드시 발생하는 경고에 대해 인지하고 있으며 경고를 띄우지 말라는 의미로 사용된다.
- deprecation, unchecked, rawtypes, varargs 와 같은 옵션을 가지고 있으며 각각 발생 원인이 되는 경고를 억제한다.
- `@SuppressWarnings({"deprecation", "rawtypes", "unchecked"})` 와 같이 여러개를 동시에 억제할 수 있다.

### @SafeVarargs
- 메서드에 선언된 가변인자의 타입이 non-reifiable 타입(제네릭 타입들) 일 경우 메서드를 선언, 호출 부분에서 `uncheked` 경고가 발생한다.
- 해당 코드에 문제가 없다면 이 경고를 억제하기 위해 이 어노테이션을 사용한다.
- 이 메서드의 가변인자는 타입 안정성이 있다고 컴파일러에게 알리는 것
- 메서드 선언에 사용하면 호출시에도 경고가 억제된다.
```java
@SafeVarargs
public static <T> void printAll(T... elements) {
    for (T element : elements) {
        System.out.println(element);
    }
}

@SafeVarargs
public static <T> void addToList(List<T> list, T... elements) {
    for (T element : elements) {
        list.add(element);
    }
}
```
- 이 메서드는 가변인자를 안전하게 처리하며, 내부에서 어떤 형변환도 발생하지 않기 때문에 @SafeVarargs를 사용해서 경고를 억제한다.
- 
## 어노테이션 선언을 위한 메타 어노테이션
```java
@Documented
@Inherited
@Target({ElementType.Type})
@Retention(RetentionPolicy.SOURCE)
public @interface AnnotaionName {
    // 애너테이션의 요소
    // 애너테이션도 상수를 정의할 수 있지만, 디폴트 메서드는 정의할 수 없다.
    int count() default 1; 
    Stirng testedBy();
    Stirng[] testTools();
    TestType TestType(); // enum 이 올 수 있다.
    DateTime testDate(); // 자신이 아닌 다른 애너테이션을 포함할 수 있다.
    
    @interface DateTime {
        String yymmdd();
        Stirng hhmmss(); 
    }
}
```
- 요소는 기본형, String, enum, class, annotation 만 허용된다.
- () 안에 매개변수를 지정할 수 없다.
- 예외를 선언할 수 없다.
- 요소를 타입 매개변수(제네릭)로 정의할 수 없다.
- 요소가 하나뿐이고 이름이 value 인경우 적용할때 요소의 이름을 생략하고 값만 적어도 된다. `@TestInfo(1)`
 
### @interface
어노테이션 선언을 위해서 사용된다.
### @Target
어떤 것에 사용되는 어노테이션인지를 나타낸다

| 적용 대상              | 설명                                 |
|----------------------|------------------------------------|
| `ElementType.ANNOTATION_TYPE` | 어노테이션 타입에 적용할 수 있습니다.  |
| `ElementType.CONSTRUCTOR`     | 생성자에 적용할 수 있습니다.         |
| `ElementType.FIELD`           | 필드(멤버 변수)에 적용할 수 있습니다.  |
| `ElementType.LOCAL_VARIABLE`  | 지역 변수에 적용할 수 있습니다.      |
| `ElementType.METHOD`          | 메서드에 적용할 수 있습니다.       |
| `ElementType.PACKAGE`         | 패키지에 적용할 수 있습니다.         |
| `ElementType.PARAMETER`       | 메서드 매개변수에 적용할 수 있습니다.    |
| `ElementType.TYPE`            | 클래스, 인터페이스(열거형 포함)에 적용할 수 있습니다.    |
| `ElementType.TYPE_PARAMETER`  | 제네릭 타입 매개변수에 적용할 수 있습니다.           |
| `ElementType.TYPE_USE`        | 타입의 사용 장소에 적용할 수 있습니다 (Java 8 이후). |
| `ElementType.MODULE`          | 모듈에 적용할 수 있습니다 (Java 9 이후).        |
| `ElementType.RECORD_COMPONENT`| 레코드 컴포넌트에 적용할 수 있습니다 (Java 16 이후). |


### @Retention
어노테이션 정보가 얼마나 유지되는지에 대한 정보

| 유지 정책 (`RetentionPolicy`) | 설명                                                                                           |
|------------------------------|------------------------------------------------------------------------------------------------|
| `SOURCE`                      | 컴파일 시 사라지며, 소스 파일에만 존재합니다. 클래스 파일에는 존재하지 않습니다.               |
| `CLASS`                       | 컴파일러가 참조하지만 JVM은 참조하지 않습니다. 클래스 파일에 존재하나, 실행 시에는 사용되지 않습니다. 기본값입니다. |
| `RUNTIME`                     | JVM이 참조하며, 클래스 파일에 존재하고 실행 시에 사용 가능합니다.                               |

### @Document
어노테이션에 대한 정보가 javadoc으로 작성한 문서에 포함되도록 한다.

### @Inherit
상속받은 자식 클래스에서 부모 클래스의 어노테이션 사용 가능 여부, 자손 클래스도 이 애너테이션이 붙은 것과 같이 인식된다.

### @Repeatable
하나의 대상에 한 종류의 애너테이션을 붙이는데, 이 어노테이션이 선언된 어노테이션은 여러번 붙일 수 있다. 
이 애너테이션들을 하나로 묶어서 다룰 수 있는 애너테이션도 추가로 정의해야한다.

### @Native
네이티브 메서드에 의해 참조되는 상수필드에 붙이는 애너테이션이다. 네이티브 메서드는 JVM 이 설치된 OS의 메서드를 말한다.
```java
@Native public static final long MIN_VALUE = 0x800000000000000L;
```

## 마커어노테이션
요소가 하나도 정의되어있지 않은 애너테이션을 마커 애너테이션이라고 한다.
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```