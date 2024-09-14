# I/O
> 입력과 출력을 통칭, JVM 기준으로 봐서 Out 이 파일로 쓰거나 외부로 전송시, In 이 읽을 때를 말한다.<br>
- 이 패키지에서는 바이트 기반의 데이터를 처리하기 위해 여러종류의 Stream 이라는 클래스를 제공한다.<br>
  - Stream은 끊기 지않고 연속적인 데이터를 말한다.
- 바이트가 아닌 char 기반의 문자열로만 되어 있는 파일은 Reader 와 Writer 라는 클래스로 처리한다.<br>
- JDK 1.4 부터는 New IO (NIO) 가 추가되었다. 스트림 기반이 아니라, 버퍼와 채널을 기반으로 데이터를 처리한다.
버퍼라는 공간에 임시로 데이터를 쌓아두고 한번에 처리해 속도가 빠르다.<br>
- Java 7 부터는 NIO2 가 추가 되었다.

# Stream
데이터를 주고받는 대상을 연결하고 전송할 수 있는 무언가를 말한다. 데이터를 운반하는데 사용되는 연결 통로이다. 단방향 통신만 가능하기 때문에, 입/출력 스트림이 별도로 필요하다.

## Byte 처리의 InputStream / OutputStream : 바이트 기반 스트림
- 둘 다 Closeable 이라는 인터페이스를 구현하는데, 어떤 리소스를 열었던 간에 이 인터페이스를 구현하면 해당 리소스는 close() 메소드를 이용해서 닫아야한다.
  - close 를 통해서 닫을때는 꼭 생성한것의 역순, 즉 마지막에 연 객체부터 닫아주어야 정상적으로 처리된다.
- OutputStream 에는 Flushable 이라는 인터페이스를 구현하는데, flush() 메소드를 구현한다.
  - 매번 쓰기 작업을 요청하면 효율이 좋지 않으니 버퍼에 차곡차곡 쌓아두었다가 한번에 쓰는데 현재 버퍼에 있는 내용을 기다리지 말고 무조건 저장하라는 메소드가 flush() 메소드이다.
1. **ByteArrayInputStream, ByteArrayOutputStream**
   - 바이트 배열(메모리영역)에 데이터를 입출력하는데 사용되는 스트림이다.
   - 다른곳에 입출력하기 전에 데이터를 임시로 바이트 배열에 담아서 변환등의 작업을 수행한다.
   - 메모리 영역만 사용하므로 가비지 컬렉터에 의해 자동으로 자원을 반납하므로 close() 가 필요하지 않다.
   - 배열을 사용해 데이터를 배열로 한번에 담아오면 작업의 효율이 상승한다.
2. **FileInputStream, FileOutputStream**
   - 파일을 입출력하기위한 스트림이다.

## Byte 기반 보조 스트림
보조 스트림은 스트림의 기능을 보완하기 위한 보조스트림으로 스트림의 기능을 향상시키거나 새로운 기능을 추가할 수 있다.
1. FilterInputStream, FilterOutputStream
   - InputStream, OutputStream 의 자손이면서 모든 보조 스트림의 조상이다.
   - 모든 메서드는 단순히 기반 스트림의 메서드를 그대로 호출할 뿌니다.
   - 상속을 통해 read(), wirte() 를 오버라이딩 해야한다.
2. BufferedInputStream / BufferedOutputStream
   - 스트림의 입출력 효율을 높이기 위해 버퍼를 사용하는 보조스트림이다.
   - read 를 호출하면 BufferedInputStream는 입력소스로부터 버퍼 크기만큼의 데이터를 묶어와 자신의 내부 버퍼에 저장한다.
   - 프로그램에서는 이 버퍼에 저장된 데이터를 읽는데 이는 외부의 입력 소스로부터 읽는것보다 훨씬 빠르다.
   - write() 할때는 버퍼가 가득차면 버퍼의 모든 내용을 출력 소스에 출력한다. 
   - 버퍼가 가득 차지 않아 버퍼에 데이터가 남아있을때, 이를 처리 하기 위해 close() 나 flush()를 호출해서 처리한다.
3. DataInputStream, DataOutputStream
   - DataInput 인터페이스를 구현했다. 데이터를 읽고 쓰는데 있어서 byte 단위가 아닌 8가지 기본 자료형의 단위로 읽고 쓸 수 있다.
   - 각 기본자료형의 값을 16진수로 표현하여 저장한다.
   - 문자를 데이터로 저장하면 다시 데이터를 읽어올 때 문자들을 실제 값으로 변환하는 과정을 거쳐야하고, 읽어야할 데이터의 개수를 결정해야하는 번거로움이 있다.
   - 하지만 DataInputStream 을 사용하면 데이터 변환도 필요없고, 자리수를 따로 세지 않아도 되므로 편리하고 빠르게 데이터를 저장하고 읽을 수 있게 된다.
4. SequenceInputStream
   - 여러개의 입력스트림을 연속적으로 연결해서 하나의 스트림으로부터 데이터를 읽는 것과 같이 처리할 수 있도록 도와준다.
   - 큰파일을 여러개의 작은 파일로 나누었다가 하나의 파일로 합치는 것과 같은 작업을 수행할 때 사용하면 좋다.
5. PrintStream
   - 데이터를 기반 스트림에 다양한 형태로 출력할 수 이쓴 pirnt, println, printf와 같은 메서드를 오버로딩하여 제공한다.

## Char 처리의 Reader / Writer : 문자 기반 스트림
- 여러 종류의 인코딩과 자바에서 사용하는 유니코드간의 변환을 자동적으로 처리해준다.
- BufferedReader 와 InputStreamReader 가 많이 사용된다.
- Writer 에는 Appendable 이라는 인터페이스가 구현되어있다. 각종 문자열을 추가하기 위한 인터페이스이다.
  - append() 라는 메소드가 존재하는데 매개 변수로 넘어온 char 을 추가하는 기능이다.
- FileWriter 를 통해서 char 기반의 내용을 파일로 슬 수 있는데 write() 나 append() 를 사용하면 메소드를 호출했을 때마다 파일에 쓰기 때문에 비효율적이다.
- BufferedWriter(Writer out) 클래스를 통해 출력하면 버퍼를 이용하므로 매우 효율적인 저장이 가능하다.
1. FileReader / FileWriter
   - 파일로부터 텍스트 데이터를 읽고, 파일에 쓰는데 사용된다.
2. PipedReader / PipedWriter
   - 스레드간에 데이터를 주고받을 때 사용된다. 
3. StringReader / StringWriter
   - CharArrayReader와 같이 입출력 대상이 메모리인 스트림이다.
   - StringWriter 에 출력되는 데이터는 내부의 StringBuffer 에 저장되며 StringWriter getBuffer(), toString() 을 통해 저장된 데이터를 얻을 수 있다.

## 문자 기반 보조 스트림
1. BufferedReader
   - 버퍼를 이용해서 입출력의 효율을 높일 수 있도록 해주는 역할을 한다.
2. InputStreamReader
   - 바이트기반 스트림을 문자기반 스트림으로 연결시켜 주는 역할을 한다.
   - 바이트 기반 스트림의 데이터를 지정된 인코딩의 문자 데이터로 변환하는 작업을 수행한다.

# 표준입출력
콘솔을 통한 데이터의 입력과 출력을 의미한다.<br>
- System.in : 콘솔로부터 데이터를 입력받는데 사용
- System.out : 콘솔로부터 데이터를 출력하는데 사용
- System.err : 콘솔로부터 데이터를 출력하는데 사용

이들은 static 변수이며 명시적인 return 타입은 InputStream, PrintStream 이지만 실제로는 버퍼를 이용하는 BufferedInputStream, BufferedOutputStream 의 인스턴스를 사용한다.<br>
setOut(), setErr(), setIn() 을 통해서 입출력을 콘솔 이외의 다른 입출력 대상으로 변경하는 것이 가능하다.

## RandomAccessFile
- 이 클래스는 하나의 클래스로 파일에 대한 입력과 출력을 모두 할 수 있도록 되어있다.
- DataInput인터페이스와 DataOutput 인터페이스를 모두 구현했다.
- 따라서 기본 자료형 단위로 데이터를 읽고 쓸 수 있다.
- 가장 큰 특징은 파일의 어느 위치에나 읽기/쓰기가 가능하다는 것이다.


## 파일을 쉽게 읽을 수 있는 Scanner 클래스
- java.util 에 있는 클래스로 텍스트 기반의 기본 자료형이나 문자열 데이터를 처리하기 위한 클래스다.
- 정규 표현식을 사용해 데이터를 잘라 처리할 수도 있다.



# File
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

