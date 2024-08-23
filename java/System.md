# System

제공하기 위해 설계되었습니다. 이 클래스는 `java.lang` 패키지에 포함되어 있으며, `final`로 선언되어 있어 상속할 수 없습니다.
대부분의 메소드는 **static**으로 선언되어 있어 객체를 생성하지 않고도 사용할 수 있습니다.

프로그램의 종료, 시간 측정, 입출력 관리 등 다양한 시스템 관련 기능을 제공합니다.
이 클래스의 대부분의 메소드는 정적 메소드로, 객체를 생성하지 않고도 사용할 수 있습니다.

1. **표준 입출력 관리**: `System.in`, `System.out`, `System.err`를 통해 표준 입력, 출력, 오류 출력 스트림에 접근할 수 있습니다.
2. **환경 변수 및 시스템 속성 관리**: 현재 시스템의 환경 변수나 속성에 접근하고 설정할 수 있습니다.
3. **시스템 시간 측정**: 현재 시간이나 경과 시간을 측정할 수 있습니다.
4. **메모리 관리**: 가비지 컬렉터를 명시적으로 호출하거나, 사용 가능한 메모리 정보를 얻을 수 있습니다.
5. **시스템 종료**: 프로그램을 종료하거나, 특정 종료 코드를 반환하며 시스템을 종료할 수 있습니다.
6. **보안 관리자 설정**: 현재 보안 관리자를 설정하거나 조회할 수 있습니다.

생성자가 없으며 3개의 스태틱 변수가 선언되어 있다.

| 선언 및 리턴 타입      | 변수명 | 설명                       |
|-----------------------|--------|----------------------------|
| `static PrintStream`   | `err`  | 에러 및 오류를 출력할 때 사용 |
| `static InputStream`   | `in`   | 입력값을 처리할 때 사용      |
| `static PrintStream`   | `out`  | 출력값을 처리할 때 사용      |

출력과 관련된 메소드들은 System 클래스가 아닌 PrintStream 클래스에서 찾아야 한다. PrintStream, InputStream은 모두 java.io 패키지에 정의되어 있다.

### 주요 메소드

| 메소드                         | 설명                                                                 |
|--------------------------------|----------------------------------------------------------------------|
| `System.currentTimeMillis()`   | 현재 시간을 밀리초 단위로 반환.                                      |
| `System.nanoTime()`            | 높은 해상도의 현재 시간(나노초 단위)을 반환.                         |
| `System.exit(int status)`      | 지정된 상태 코드와 함께 Java 가상 머신을 종료.                        |
| `System.gc()`                  | 가비지 컬렉션을 요청.                                                |
| `System.getenv(String name)`   | 지정된 환경 변수의 값을 반환.                                        |
| `System.getProperties()`       | 현재 시스템의 속성들을 Properties 객체로 반환.                        |
| `System.getProperty(String key)`| 지정된 키에 대한 시스템 속성을 반환.                                 |
| `System.setProperty(String key, String value)`| 지정된 키에 대한 시스템 속성을 설정.                    |
| `System.identityHashCode(Object x)` | 객체의 고유한 해시 코드를 반환 (기본 해시 코드를 무시).            |
| `System.load(String filename)` | 지정된 파일 이름의 네이티브 라이브러리를 로드.                      |
| `System.loadLibrary(String libname)` | 지정된 라이브러리 이름의 네이티브 라이브러리를 로드.             |
| `System.runFinalization()`     | 종료 후 모든 객체의 finalization을 실행.                             |
| `System.setSecurityManager(SecurityManager s)` | 현재 보안 관리자를 설정.                             |
| `System.getSecurityManager()`  | 현재 보안 관리자를 반환.                                            |
| `System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length)` | 배열의 특정 영역을 복사. |
| `System.in`                    | 표준 입력 스트림을 참조하는 InputStream 객체.                         |
| `System.out`                   | 표준 출력 스트림을 참조하는 PrintStream 객체.                         |
| `System.err`                   | 표준 오류 출력 스트림을 참조하는 PrintStream 객체.                    |


## Properties 클래스
Properties 클래스는 자바에서 키-값 쌍으로 구성된 속성을 관리하기 위한 클래스로, 주로 애플리케이션의 설정 정보를 관리하는 데 사용
Hashtable을 상속받아 구현되었으며, 키와 값 모두 문자열(String)로 제한된다는 점에서 일반적인 Map과 다릅니다.

- 문자열 기반의 키-값 쌍 관리: Properties 클래스의 모든 키와 값은 문자열(String)로 관리됩니다.
- 파일 입출력 지원: .properties 파일을 통해 애플리케이션 설정을 쉽게 로드하고 저장할 수 있습니다.
- 기본값 지원: 기본 속성(Properties 객체)을 설정하여, 속성을 가져올 때 값이 없으면 기본값을 참조하도록 할 수 있습니다.

System 클래스는 애플리케이션의 시스템 속성을 관리하는데, 이 시스템 속성은 내부적으로 Properties 클래스로 관리됩니다.
System 클래스는 기본적으로 자바 런타임 환경의 다양한 정보를 담고 있는 Properties 객체를 가지고 있으며,
이 객체를 통해 시스템 속성에 접근하거나 수정할 수 있습니다.

```java
import java.util.Properties;

public class SystemPropertiesExample {
    public static void main(String[] args) {
        // System 클래스를 통해 시스템 속성에 접근
        Properties systemProps = System.getProperties();
        
        // 시스템 속성 출력
        systemProps.list(System.out);

        // 특정 시스템 속성 접근
        String javaVersion = System.getProperty("java.version");
        String osName = System.getProperty("os.name");
        
        System.out.println("Java Version: " + javaVersion);
        System.out.println("Operating System: " + osName);
        
        // 시스템 속성 수정
        System.setProperty("customProperty", "customValue");

        // 수정된 시스템 속성 확인
        String customProperty = System.getProperty("customProperty");
        System.out.println("Custom Property: " + customProperty);
    }
}

```