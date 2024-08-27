# list 
- List 는 배열과 같이 순서가 있다.
- List 인터페이스를 구현한 클래스로는 java.util 의 ArrayList, LinkedList, Vector 가 있다.
- ArrayList 와 Vector 는 유사하나 ArrayList 는 스레드에 안전하지 못하고 Vector 는 안전하다. 이 둘은 모두 확장 가능한 배열이다.
- Vector 클래스를 확장해 Stack 클래스를 만들었다.
- size() 메소드를 통해 객체의 크기를 int 로 확인 가능
- ArrayList, Vector 는 데이터를 읽어오고 저장하는데는 효율이 좋지만 용량을 변경해야할 때는 새로운 배열을 생성한 후 기존의 배열로부터 새로 생성된 배열로 데이터를 복사해야하기 때문에 효율이 떨어진다.

## ArrayList
- Object 배열을 이용해서 데이터를 순차적으로 저장한다. 배열에 더이상 저장할 공간이 없으면 보다 큰 새로운 배열을 생성해 복사후 저장한다.
- 따라서 ArrayList 생성시에는 저장할 요소의 개수를 고려해서 실제 저장할 개수보다 약간 여유있는 크기로 하는 것이 좋다. 복사 후 저장 방식에 처리 시간이 많이 소요된다.
- 기본 생성자로 생성시 10개의 저장공간을 갖는다. 매개변수에 int 를 넣으면 해당 int 값 만큼의 저장공간을 가진 ArrayList 가 만들어진다.
- 리스트에는 한가지 종류의 객체만 저장하기 때문에 제네릭을 사용해서 타입을 미리 지정한다.
  - JDK7부터는 타입 없이 <> 만 사용해도 가능하다.
- 리스트 복제시에는 swallow copy 와 deep copy 가 있다.
  - swallow copy : 참조를 복사하여 원본과 복사본이 동일한 객체를 가리킵니다.
    - ```java
            ArrayList<String> originalList = new ArrayList();
            list.add("A");
            list.add("B");
            ArrayList<String> shallowCopy1 = new ArrayList();
            shallowCopy1 = originalList;
      
            //// 두 리스트는 동일한 객체를 참조
            System.out.println(originalList == shallowCopy1); // true
      
            //또는
            ArrayList<String> shallowCopy2 = (ArrayList<String>) originalList.clone();
      
            // 두 리스트는 같은 객체를 참조
            System.out.println(originalList == shallowCopy2); // false
            System.out.println(originalList.get(0) == shallowCopy2.get(0)); // true
      ```
  - deep copy : 객체 자체를 복사하여 원본과 복사본이 서로 다른 객체를 가리킵니다.
    - ```java
        ArrayList<Item> deepCopy = new ArrayList<>();
        for (Item item : originalList) {
        try {
            deepCopy.add((Item) item.clone());
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
            }
        }
        
        // 두 리스트는 서로 다른 객체를 참조
        System.out.println(originalList == deepCopy); // false
        System.out.println(originalList.get(0) == deepCopy.get(0)); // false
      ```

## LinkedList
```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable{}
```
- AbstractSequentialList 를 확장함
- 배열의 단점을 보완하기 위해서 고안된 자료 구조
  - 크기를 변경할 수 없다.
  - 비순차적인 데이터의 추가, 삭제에 시간이 많이 걸림
- 링크드 리스트의 각 요소들은 자신과 연결된 다음 요소에 대한 참조 주소값과 데이터로 구성되어있다.
- 데이터 추가, 삭제시 복사 과정없이 하나의 참조만 변경하면 되므로 처리 속도가 빠르다.
- 기존 링크드 리스트는 다음 데이터에 대한 참조만 있어 한방향으로만 가지만 이를 극복하기 위해 더블 링크드 리스트가 나왔다.
- 더블 링크드 리스트는 이전 참조변수를 참조한다.
- 순차적으로 추가/삭제하는 경우는 어레이리스트가 더 빠르다. 
  - 삭제시 어레이는 제일 마지막것만 null 로 변경하면 된다.
  - 추가시 초기 용량이 충분하다면 어레이가 더 빠르지만 용량을 넘어간다면 링크드가 더 빠를 수도 있다.
- 중간 데이터의 추가/삭제 하는 경우는 링크드 리스트가 더 빠르다.
- 인덱스가 n 인 요소의 값을 얻어올때,배열은 각 요소들이 연속적으로 각각의 주소를 가지고 메모리상에 존재하기 때문에 간단히 가져올수있다.
- 하지만 링크드리스트는 불연속적으로 위치한 요소들이 서로 연결된 것이라 차례대로 n 번째 데이터까지 접근해야한다. 데이터가 많을수록 요소를 얻어오는 시간이 증가한다.

## Vector 를 구현한 Stack
- LIFO 를 위해 만들어졌지만 이것보다 빠른 ArrayDeque(스레드에 안전하지 x) 가 있다.
- peek() : 가장 위에 있는 데이터를 리턴만한다.
- pop() : 가장 위에 있는 데이터를 리턴하고 지운다.
- push() : 매개변수로 넘어온 데이터를 가장 위에 저장한다.
- search() : 매개변수로 들어온 데이터의 위치를 리턴한다.


## LinkedList 를 구현한 Queue
