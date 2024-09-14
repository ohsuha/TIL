# serializable
>자바에서 객체를 직렬화할 수 있게 해주는 인터페이스이다. 직렬화란 객체를 바이트 스트림으로 변환하여 저장하거나 전송할 수 있도록 하는 과정이다. 이 인터페이스를 구현하면 객체를 파일에 저장하거나 네트워크를 통해 전송할 수 있다.

- 이 인터페이스는 메서드가 없고 단순히 이 인터페이스를 구현함으로써 객체가 직렬화될 수 있다는 것을 표시한다.
  - Serializable을 구현하지 않으면 객체를 직렬화할 수 없고, java.io.NotSerializableException이 발생할 수 있다.
- serialVersionUID : 직렬화된 객체의 버전을 식별하는 UID(Unique Identifier)이다.
  - 객체의 버전이 호환되는지 확인할 때 사용된다. 클래스가 변경되면 serialVersionUID를 명시적으로 업데이트해야 할 수 있다.
- 클래스가 변경되면 serialVersionUID를 업데이트하여 호환성을 유지해야 한다. 그렇지 않으면 InvalidClassException이 발생할 수 있다.

Serializable은 자바에서 객체를 바이트 스트림으로 직렬화할 수 있게 해주는 인터페이스이다.
이 인터페이스를 구현하면 객체를 파일에 저장하거나 네트워크를 통해 전송할 수 있다.
직렬화와 역직렬화는 ObjectOutputStream과 ObjectInputStream 클래스를 통해 처리할 수 있으며,
serialVersionUID를 사용하여 객체의 버전을 식별하고 호환성을 유지할 수 있다.

## 직렬화와 역직렬화?
- 직렬화 : ObjectOutputStream 클래스를 사용하여 객체를 바이트 스트림으로 변환할 수 있다. 객체를 바이트 스트림으로 변환해서 저장하거나 전송할 수 있게 하는 과정
- 역직렬화 : ObjectInputStream 클래스를 사용하여 바이트 스트림을 다시 객체로 변환할 수 있다. 역직렬화는 바이트 스트림을 다시 원래의 객체로 변환하는 과정

## transient
- 직렬화를 제외할 필드는 transient 키워드를 사용하여 표시할 수 있다.

