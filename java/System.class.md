# System
> Java의 표준 라이브러리인 `java.lang` 패키지에 속하며, Java 프로그램의 기본적인 시스템 관련 작업을 수행하는 데 필요한 다양한 유틸리티 메소드를 제공함. 이 클래스는 모든 필드와 메소드가 `static`으로 선언되어 있어, 인스턴스를 생성하지 않고도 직접 호출이 가능함.

`System` 클래스는 Java 프로그램이 실행되는 환경에 대한 중요한 정보를 제공하며, 프로그램이 해당 환경과 상호 작용할 수 있는 수단을 제공함. 이를 통해 프로그램이 표준 입출력 작업을 수행하고, 시스템 속성이나 환경 변수를 조회하거나 설정하며, 프로그램의 실행 상태를 제어할 수 있게 됨. 또한, 이 클래스는 일반적으로 자주 사용되는 기능들을 모아두어 개발자가 쉽게 접근할 수 있도록 함으로써 개발의 효율성을 높이는 역할을 함.
### 주요 메소드와 기능

#### 1. `System.out`, `System.err`, `System.in`
- **`System.out`**: 표준 출력 스트림을 나타냄. 일반적으로 콘솔에 데이터를 출력하는 데 사용됨.
  ```java
  System.out.println("Hello, World!");
  ```
- **`System.err`**: 표준 오류 출력 스트림을 나타냄. 오류 메시지를 출력할 때 사용됨.
  ```java
  System.err.println("An error occurred.");
  ```
- **`System.in`**: 표준 입력 스트림을 나타냄. 사용자로부터 데이터를 입력받을 때 사용됨.
  ```java
  Scanner scanner = new Scanner(System.in);
  String input = scanner.nextLine();
  ```

#### 2. `System.currentTimeMillis()`
- 이 메소드는 현재 시간을 밀리초 단위로 반환함. 주로 성능 측정이나 타이머 기능 구현 시 사용됨.
  ```java
  long startTime = System.currentTimeMillis();
  // 작업 수행
  long endTime = System.currentTimeMillis();
  System.out.println("Elapsed time: " + (endTime - startTime) + "ms");
  ```

#### 3. `System.nanoTime()`
- 보다 정확한 시간 측정을 위해 현재 시간을 나노초 단위로 반환함. 상대적 시간 측정에 적합하며, 절대적인 날짜나 시간을 의미하지 않음.
  ```java
  long startNano = System.nanoTime();
  // 작업 수행
  long endNano = System.nanoTime();
  System.out.println("Elapsed time: " + (endNano - startNano) + "ns");
  ```

#### 4. `System.exit(int status)`
- 프로그램을 종료시키는 메소드로, 인수로 전달된 상태 코드를 반환하며 JVM을 종료함. `0`은 정상 종료를 의미하고, 비정상 종료 시에는 다른 숫자를 사용함.
  ```java
  System.exit(0);
  ```

#### 5. `System.gc()`
- 가비지 컬렉터를 실행하도록 요청하는 메소드임. 그러나 호출한다고 해서 즉시 가비지 컬렉션이 실행된다는 보장은 없음.
  ```java
  System.gc();
  ```

#### 6. `System.getenv()`
- 시스템의 환경 변수를 읽어오는 메소드로, 환경 변수의 값이나 전체 환경 변수를 맵(Map) 형태로 반환함.
  ```java
  String path = System.getenv("PATH");
  ```

#### 7. `System.getProperty()`, `System.setProperty()`
- **`getProperty(String key)`**: 시스템 속성을 가져오는 메소드로, 예를 들어 Java 버전이나 운영체제 이름 등을 확인할 수 있음.
  ```java
  String javaVersion = System.getProperty("java.version");
  ```
- **`setProperty(String key, String value)`**: 시스템 속성을 설정하는 메소드임.
  ```java
  System.setProperty("myProperty", "myValue");
  ```