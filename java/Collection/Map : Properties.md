# Properties
- Hashtable 을 상속받아 구현한 것으로 (String, String) 의 형태로 저장한다.
- 주로 애플리케이션의 환경설정과 관련도니 속성을 저장하는데 사용되며 데이터를 파일로부터 읽고 쓰는 편리한 기능을 제공한다.
- Hashtable 을 상속받아 구현한 것이라 컬렉션 프레임웍 이전의 구버전이므로 Iterator 가 아닌 Enumeration 을 사용한다.

## 주요 메소드
| 메서드                        | 설명                                                        | 사용 예제                                              |
|-------------------------------|-------------------------------------------------------------|--------------------------------------------------------|
| `load(InputStream inStream)`  | 입력 스트림에서 속성을 읽어들입니다.                        | `properties.load(new FileInputStream("config.properties"));` |
| `store(OutputStream out, String comments)` | 속성을 출력 스트림에 저장합니다.                        | `properties.store(new FileOutputStream("config.properties"), "Comments");` |
| `getProperty(String key)`      | 주어진 키에 대한 값을 반환합니다.                           | `String value = properties.getProperty("key");`        |
| `setProperty(String key, String value)` | 주어진 키와 값으로 속성을 설정합니다.                   | `properties.setProperty("key", "value");`              |
| `remove(Object key)`           | 주어진 키에 해당하는 속성을 삭제합니다.                    | `properties.remove("key");`                           |
| `list(PrintStream out)`        | 속성을 출력 스트림에 나열합니다.                           | `properties.list(System.out);`                         |
| `stringPropertyNames()`        | 속성의 모든 키를 문자열 집합으로 반환합니다.               | `Set<String> keys = properties.stringPropertyNames();`  |
| `clear()`                      | 모든 속성을 제거합니다.                                    | `properties.clear();`                                 |

- 데이터에 한글이 들어갈 경우 storeToXML()을 이용해서 가져오자