## ArrayList, Vector
```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```
- ArrayList 는 Vector 를 개선한 것으로 구현원리와 기능적인 측면에서 동일하다.
  - 단 Vector 는 스레드에 안전하지만 ArrayList 는 그렇지 않다.
- Object 배열을 이용해서 데이터를 순차적으로 저장한다. 배열에 더이상 저장할 공간이 없으면 보다 큰 새로운 배열을 생성해 복사후 저장한다.
  - 따라서 ArrayList 생성시에는 저장할 요소의 개수를 고려해서 실제 저장할 개수보다 약간 여유있는 크기로 하는 것이 좋다. 복사 후 저장 방식에 처리 시간이 많이 소요된다.
  - Object 배열을 사용하는 이유는 E[] 와 같이 배열을 제네릭으로 선언할 수 없기 때문이다.
  - `T[] r = a.length >= size ? a : (T[]) java.lang.reflect.Array.newInstance(a.getClass().getComponentType(), size);`
  - 제네릭 이레이저로 인해  toArray(T[] a) 배열을 생성할때 실제 타입 정보를 유지할 수 없다. 
  - java.lang.reflect.Array.newInstance()를 사용하여 런타임에 정확한 배열 타입을 생성해야 한다.
  - 이 과정에서 (T[])와 같은 캐스팅이 필요합니다.
- 리스트에는 한가지 종류의 객체만 저장하기 때문에 제네릭을 사용해서 타입을 미리 지정한다.
  - JDK7부터는 타입 없이 <> 만 사용해도 가능하다.
- 데이터를 읽어오고 저장하는데는 효율이 좋지만 용량을 변경해야 할 때는 새로운 배열을 생성한 후 기존의 배열로부터 새로 생성된 배열로 데이터를 복사해야하기 때문에 효율이 떨어진다.
- 기본 생성자로 생성시 10개의 저장공간을 갖는다. 매개변수에 int 를 넣으면 해당 int 값 만큼의 저장공간을 가진 ArrayList 가 만들어진다.
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
          
## 메소드

| 메소드 | 설명 |
|--------|------|
| `Object clone()` | 리스트의 얕은 복사본을 반환. |
| `void ensureCapacity(int minCapacity)` | 리스트의 용량을 최소한 지정된 크기로 증가시킴. |
| `int lastIndexOf(Object o)` | 리스트에서 지정된 요소의 마지막 발생 위치를 반환. |
| `void trimToSize()` | 리스트의 용량을 현재 크기에 맞게 조정. |
