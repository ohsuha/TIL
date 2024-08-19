# list 
- List 는 배열과 같이 순서가 있다.
- List 인터페이스를 구현한 클래스로는 java.util 의 ArrayList, LinkedList, Vector 가 있다.
- ArrayList 와 Vector 는 유사하나 ArrayList 는 스레드에 안전하지 못하고 Vector 는 안전하다. 이 둘은 모두 확장 가능한 배열이다.
- Vector 클래스를 확장해 Stack 클래스를 만들었다.
- size() 메소드를 통해 객체의 크기를 int 로 확인 가능
- ArrayList, Vector 는 데이터를 읽어오고 저장하는데는 효율이 좋지만 용량을 변경해야할 때는 새로운 배열을 생성한 후 기존의 배열로부터 새로 생성된 배열로 데이터를 복사해야하기 때문에 효율이 떨어진다.

## ArrayList
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

## Vector 를 구현한 Stack
- LIFO 를 위해 만들어졌지만 이것보다 빠른 ArrayDeque(스레드에 안전하지 x) 가 있다.
- peek() : 가장 위에 있는 데이터를 리턴만한다.
- pop() : 가장 위에 있는 데이터를 리턴하고 지운다.
- push() : 매개변수로 넘어온 데이터를 가장 위에 저장한다.
- search() : 매개변수로 들어온 데이터의 위치를 리턴한다.



