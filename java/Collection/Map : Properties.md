# Properties
- Hashtable 을 상속받아 구현한 것으로 (String, String) 의 형태로 저장한다.
- 주로 애플리케이션의 환경설정과 관련도니 속성을 저장하는데 사용되며 데이터를 파일로부터 읽고 쓰는 편리한 기능을 제공한다.
- Hashtable 을 상속받아 구현한 것이라 컬렉션 프레임웍 이전의 구버전이므로 Iterator 가 아닌 Enumeration 을 사용한다.

## 주요 메소드

- 데이터에 한글이 들어갈 경우 storeToXML()을 이용해서 가져오자