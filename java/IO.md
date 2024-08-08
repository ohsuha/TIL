# I/O
> 입력과 출력을 통칭, JVM 기준으로 봐서 Out 이 파일로 쓰거나 외부로 전송시, In 이 읽을 때를 말한다.<br>

- 이 패키지에서는 바이트 기반의 데이터를 처리하기 위해 여러종류의 Stream 이라는 클래스를 제공한다.<br>
  - Stream은 끊기 지않고 연속적인 데이터를 말한다.
- 바이트가 아닌 char 기반의 문자열로만 되어 있는 파일은 Reader 와 Writer 라는 클래스로 처리한다.<br>
- JDK 1.4 부터는 New IO (NIO) 가 추가되었다. 스트림 기반이 아니라, 버퍼와 채널을 기반으로 데이터를 처리한다.
버퍼라는 공간에 임시로 데이터를 쌓아두고 한번에 처리해 속도가 빠르다.<br>
- Java 7 부터는 NIO2 가 추가 되었다.

## File
- File은 파일만 가리키는 것이 아니라 파일의 경로 정보도 포함한다.
- OS별로 디렉터리 구분 기호가 다르기 때문에 이를 해결하기 위해 separator 라는 것이 static 변수로 존재한다.
  - `File.separator`
- mkdir() : 매개변수로 들어온 디렉토리 하나만 만든다.
- mkdirs() : 존재하지 않는 여러 디렉토리를 한 번에 만든다.
- `get..Path()` 메소드를 통해 경로를 얻을 수 있다
  - Absolute 경로는 절대 경로로 `C:\godofjava\a\..b` 
  - Canonical 은 절대적이고 유일한 경로를 의미한다. `C:\godofjava\b` 가 된다.
  - `getParent()` 를 통해 해당 파일이름을 제외한 경로만 얻을 수 있다.
- `FileFilter` 와 `FilenameFilter` 인터페이스를 구현해서 파일을 필터링 할 수 있다.
  - 내부의 accept(File file) 메소드를 구현해서 사용한다.
  - FilenameFilter : 파일과 디렉토리를 구분하지 못한다.
  - FileFilter : 파일을 구분해서 파일만 필터링한다.

## Files 클래스
- File 클래스는 정체가 불분명하고, 심볼릭 링크와 같은 유닉스 계열의 파일에서 사용하는 기능을 제대로 제공하지 못한다.
- Java7부터는 NIO2의 Files 클래스를 통해 이 문제를 해결했다.
- Files 는 모든 메소드가 static 으로 선언되었다. 
- `String data = new String(Files.readAllBytes(Paths.get(filName)));` 을 통해서 파일을 읽어올 수 있다.

## Byte 처리의 InputStream / OutputStream
- 둘 다 Closeable 이라는 인터페이스를 구현하는데, 어떤 리소스를 열었던 간에 이 인터페이스를 구현하면 해당 리소스는 close() 메소드를 이용해서 닫아야한다.
  - close 를 통해서 닫을때는 꼭 생성한것의 역순, 즉 마지막에 연 객체부터 닫아주어야 정상적으로 처리된다.
- OutputStream 에는 Flushable 이라는 인터페이스를 구현하는데, flush() 메소드를 구현한다.
  - 매번 쓰기 작업을 요청하면 효율이 좋지 않으니 버퍼에 차곡차곡 쌓아두었다가 한번에 쓰는데 현재 버퍼에 있는 내용을 기다리지 말고 무조건 저장하라는 메소드가 flush() 메소드이다.

## Char 처리의 Reader / Writer
- BufferedReader 와 InputStreamReader 가 많이 사용된다.
- Writer 에는 Appendable 이라는 인터페이스가 구현되어있다. 각종 문자열을 추가하기 위한 인터페이스이다.
  - append() 라는 메소드가 존재하는데 매개 변수로 넘어온 char 을 추가하는 기능이다.
- FileWriter 를 통해서 char 기반의 내용을 파일로 슬 수 있는데 write() 나 append() 를 사용하면 메소드를 호출했을 때마다 파일에 쓰기 때문에 비효율적이다.
- BufferedWriter(Writer out) 클래스를 통해 출력하면 버퍼를 이용하므로 매우 효율적인 저장이 가능하다.

## 파일을 쉽게 읽을 수 있는 Scanner 클래스
- java.util 에 있는 클래스로 텍스트 기반의 기본 자료형이나 문자열 데이터를 처리하기 위한 클래스다.
- 정규 표현식을 사용해 데이터를 잘라 처리할 수도 있다.