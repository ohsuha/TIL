# NIO (new I/O)
> Java NIO (New Input/Output)는 Java 1.4에서 도입된 API로, 효율적인 I/O(입출력) 작업을 지원하기 위해 설계되었다.
> NIO는 블로킹과 논블로킹 모드를 지원하며, 특히 대량의 데이터를 처리하거나 네트워크 프로그래밍에서 성능을 향상시킬 수 있는 기능을 제공한다.

- java.nio.Buffer 클래스를 확장하여 사용한다.

## Channels 
- 데이터의 읽기와 쓰기를 위한 통로 역할을 한다. NIO에서는 스트림과는 달리 양방향 통신을 지원하는 채널을 사용한다.
- 주요 채널 클래스: FileChannel, SocketChannel, ServerSocketChannel, DatagramChannel

## Buffer
> Buffer는 NIO의 핵심 클래스로, 데이터를 일시적으로 저장하는데 사용된다.
- 채널에서 데이터를 읽고 쓸 때 사용하는 컨테이너입니다. 버퍼는 고정된 크기를 가지며, 읽기와 쓰기를 위한 인덱스를 관리합니다.
- Buffer 클래스는 추상 클래스이며, ByteBuffer, CharBuffer, IntBuffer 등과 같은 구체적인 하위 클래스가 있다.<br> 
- 각 버퍼 클래스는 특정 데이터 타입을 처리하도록 설계되어 있다.
- 주요 버퍼 클래스: ByteBuffer, CharBuffer, IntBuffer, FloatBuffer



