# generic
> test code : https://github.com/ohsuha/code_scratch/blob/master/code_scratch/src/test/java/GenericsTest.java
> https://github.com/ohsuha/code_scratch/tree/master/code_scratch/src/main/java/test/generics

제네릭은 형변환에 생길 수 있는 문제점을 컴파일 할 때 정정할 수 있도록 만들어졌다.

- 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시의 타입 체크를 해주는 기능이다.
- 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.
  - 타입 안정성을 높인다 : 의도하지 않은 타입의 객체가 저장되는 것을 막고, 저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄여준 다는 뜻이다.
  - 형변환의 번거로움이 줄어든다 : 꺼낼 때마다 타입체크를 하고, 형변환을 하는 것이 불편하다. 또 원치 않는 객체가 포함되는 것을 막을 방법이 없는데 이를 해결해준다는 뜻이다.
- 다룰 객체의 타입을 미리 명시해줌으로써 번거로운 형변환을 줄여준다.
- - 기존에는 다양한 종류의 타입을 다루는 메서드의 매개변수나 리턴타입으로 Object 를 사용해 형변환이 불가피 했지만, 원하는 타입을 지정하기만 하면 된다.

## generic class
![img](https://blog.kakaocdn.net/dn/cKJc01/btqv0yfQWxl/xyKRLszrlst7OlNRruz620/img.png)
- 명칭
  - TestGeneric<T\> : 지네릭 클래스, T의 TestGeneric, T TestGeneric 이라 읽는다.
  - T : 타입 변수, 타입 매개변수
  - TestGeneric : 원시 타입
- 클래스에 선언할 때는 `className<T>` 와 같이 선언하는데 이 괄호 안에는 실제 존재하는 클래스를 사용해도 되고 존재 하지 않는 것을 사용해도 된다.
- 꺽쇠 안에 선언된 이름은 클래스 안에서 하나의 타입처럼 사용된다. 가상의 타입 이름이라고 생각하자.
- 타입 매개변수에 타입을 지정하는 것을 `지네릭 타입 호출`, T에 String 같은걸 넣으면 `매개변수화된 타입, 대입된 타입` 이라고 한다.
- 컴파일 후에는 지네릭 타입이 제거되고 원시 타입인 TestGeneric로 바뀐다.

### 제한
- TestGeneric 의 객체를 생성할 때, 객체별로 다른 타입을 지정하는 것은 가능하다. 그러라고 만든 것임
  - ```java
    Box<Apple> appleBox = new Box<Apple>();
    Box<Banana> bananaBox = new Box<Banana>();
    ```
- 모든 객체에 대해 동일하게 동작해야하는 static 멤버에 타입 변수 T를 사용할 수 없다. T는 인스턴스 변수로 간주되기 때문이다.
  - ```java
    class Box<T> {
       static T item; //error
       static int compare(T t1, T t2) { // error
          ...
       }
    }   
    ```
  - static 멤버는 타입 변수에 지정된 타입, 즉 대입된 타입 종류에 관계 없이 동일한 것이어야 하기 때문이다.
  - Box<Apple>.item 이 Box<Banana>.item 과 다른 것이어서는 안된다.
- 지네릭 타입의 배열을 생성하는 것도 허용되지 않는다.
  - 지네릭 배열 타입의 참조변수를 선언하는 것은 가능하지만. `new T[10]` 과 같이 생성할 수 없다.
  - ```java
    Class Box<T> {
        T[] itemArr; //ok
        T[] toArray() {
            T[] tmpArr = new T[itemArr.length]; // error 제네릭 배열 생성 불가
            ...
            return tmpArr;
        }
    ```
  - new 연산자 때문인데, 컴파일 시점에 타입 T가 뭔지 정확히 알아야한다. 그런데 Box<T> 를 컴파일하는 시점에서는 T가 어떤 타입이 될지 알수 없다.
  - `instanceOf()` 도 같은 이유로 T 연산자를 사용할 수 없다.
  - 꼭 사용해야한다면 new 대신 reflection 의 newInstance(), 또는 Object 배열을 생성해 복사한 후 T[] 로 형변환 해야한다.

### 상속 관계에서의 사용
- 두 지네릭 타입이 상속관계에 있어도 대입된 타입이 다르면 에러가 발생한다.
- 하지만 두 지네릭 타입이 상속관계에 있고, 대입된 타입이 같은것은 괜찮다.
```java
FruitBox extends Box 일 때
Box<Fruit> appleBox = new Box<Apple>(); //error
Box<Apple> appleBox = new FruitBox<Apple>(); //ok! 다형성
```
- 타입 T 가 부모일때, add 메서드에서 자식 타입을 받을 수 있다.
```java
class Box<T> {
    ArrayList<T> list = new ArrayList<T>();
    void add(T item) {list.add(item);}
}

class Apple extends Fruit {...}

----------------------------------------------

Box<Fruit> fruitBox = new Box<Fruit>();
fruitBox.add(new Fruit());
fruitBox.add(new Apple()); //ok
```

### extends
- 지네릭 타입에 `T extends Fruit` 과 같이 사용하면 Fruit 타입의 자손들만 들어갈 수 있도록 제한할 수 있다.
- 클래스가 아니라 인터페이스일 떄도 `extends` 를 사용한다.
- 클래스의 자손이면서 인터페이스도 구현해야한다면 `&` 기호로 연결한다
  - `class FruitBox<T extends Fruit & Eatable> {...}`

## generic method
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPKxdy%2FbtqG9A6wj3r%2F45am1mQxCFFbdUUvUCrh00%2Fimg.png)
- 메서드의 선언부에 지네릭 타입이 선언된 메서드를 말한다. 대표적으로 `Collections.sort()` 가 있다.
- 두개 이상의 제네릭이 필요할 경우 <T,S>, <T, S extends String> 과 같이 사용할 수 있다.
- 지네릭 클래스에 정의된 타입 매개변수와 메서드에 정의된 타입 매개변수는 전혀 별개의 것이다. 같은 T를 사용해도 다른것이므로 주의하자
- static 멤버에는 타입 매개 변수를 사용할 수 없지만, 메서드에는 지네릭 선언이 가능하다.
  - 지역 변수와 다름없이 사용한다고 생각이 타입 매개변수는 메서드 내에서만 지역적으로 사용하기 때문에 static 이든 아니든 상관이 없다.
  - `Juicer.<Fruit>makeJuice(fruitBox)` 와 같이 사용할 수 있다. <> 생략 가능
- 매개변수의 타입이 복잡할 때도 유용하다.
  - ```java
    public static void printAll(ArrayList<? extends Product> list){};
    아래처럼 변환할 수 있다!!!
    public static <T extends Product> void printAll(ArrayList<T> list){}; 
    ```

### wild card
- 메소드에서 제네릭 타입을 명시적으로 지정하기 애매할 경우에는 <> 안에 ? 라는 와일드 카드를 넣을 수 있다.
  - 단 이 경우 메소드내부는 해당 타입을 모르기 때문에 Object 로 처리 해야 한다.
- 지네릭 타입이 다른 것 만으로는 오버로딩이 성립하지 않는다. 이럴때 사용하기 위해 고안된 것이 와일드카드이다.
- 와일드카드는 메개변수에만 사용하는 것이 좋다. 어떤 객체를 wildcard 로 선언하고, 그 객체의 값을 가져올 순 있지만 와일드 카드로 객체를 선언했을때 특정 타입으로 값을 지정하는 것이 불가능해진다.
  - `? extends Fruit` : Fruit 와 그 자손만 들어올 수 있다. 상한 제한
  - `? super Apple` : Apple 과 그 조상들만 가능하다. 하한 제한. 비교 대상이 되는 멤버변수가 부모에 있다면 Grape 의 weight 도 가져올 수 있다.
  - `?` : 제한 없음. 모든 타입이 가능하다 `? extends Object` 와 같다.
- `public static <T extends Comparable<? super T>> void sort (List<T> list)`
  - `List<T>` : 타입 T 를 요소로하는 List를 매개변수로 이용한다.
  - `<T extends Comparable<? super T>>` : List<T> 의 요소가 Comparable 인터페이스를 구현한 것이어야 한다는 뜻이다.
    1. T extends : T는 Comparable을 구현한 클래스이어야 한다.
    2. Comparable<? super T> : T 또는 그 조상의 타입을 비교하는 Comparable 이어야한다.
    3. T가 Student 이고 Person의 자손이면 Comparable 을 구현한 Student, Person, Object 가 올수있다.
    4. 만약 Person의 자식으로 '회사원' 이있고 person의 name 이 있다면 name 을통해 Comparable 의 구현한 메소드를 사용할 수 있다.

## generic 타입의 형변환
- 지네릭 타입과 원시 타입(raw type) 간의 형변환은 가능하다.
- 단 대입된 타입이 다른 지네릭 타입간의 형변환은 불가능하다.
- ? 단독 와일드카드로 선언된 지네릭타입은 형변환이 가능하다. 대신 확인되지 않은 타입으로의 형변환이라는 경고가 뜨긴 뜬다.
  - 와일드 카드는 타입이 확정된 타입이 아니므로 컴파일러가 미확정 타입으로 형변환 하는 것이라고 경고한다.

## generic 타입의 제거
컴파일러는 지네릭 타입을 이용해서 소스파일을 체크하고, 필요한 곳에 형변환을 넣어준다. 그리고 지네릭 타입을 제거한다.
즉, 컴파일된 class 파일에는 지네릭 타입에 대한 정보가 없다.
1. 지네릭 타입의 경계를 제거한다.
   - ```java
      class Box <T extends Fruit> {
        void add(T t){}
      }
      
      class Box {
        void add(Fruit t){}
      }
      ```
2. 지네릭 타입을 제거한 후에 타입이 일치하지 않으면 형변환을 추가한다.
   - ```java
     T get (int i) {return list.get(i);}
     
     Fruit get (int i) {return (Fruit)list.get(i);}
     ```
3. 와일드카드가 포함되어있는 경우에는 알아서 적절한 타입으로 형변환이 추가된다.
   - ```java
     static Juice makeJuice(FruitBox<? extends Fruit> box){
        String tmp = "";
        for(Fruit f : box.getList()) tmp += f;
        return new Juice(tmp);
     }
     
     static Juice makeJuice(FruitBox box){
        String tmp = "";
        Iterator it = box.getList().iterator();
        while(it.hasNext()) {
            tmp += (Fruit)it.next();
        }
        return new Juice(tmp);
     }
     ```
     
### 제네릭 이레이저
`T[] = new T[]`를 사용할 수 없는 이유
제네릭 타입 정보는 컴파일 후 제거(소거)됩니다. 이때 T와 같은 제네릭 타입은 실제 구체적인 타입으로 대체되거나, Object로 변환
```java
public class Box<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }
}
```
이런 코드가 있으면 컴파일 할때는 그냥 Box 라는 클래스가 됨

제네릭은 다음의 두가지 사항이 있다.
1. 타입 안정성 보장: 제네릭은 컴파일 시 타입 안정성을 보장합니다. new T[]를 허용할 경우, 컴파일러는 T가 어떤 타입인지 확신할 수 없기 때문에 타입 안전성을 보장할 수 없습니다.
2. 배열은 런타임에 타입 정보를 유지해야 함: 배열은 런타임에 자신이 어떤 타입의 요소를 갖는지를 알아야 합니다. 예를 들어, String[] 배열은 오직 String 타입의 요소만 가질 수 있습니다. 하지만 제네릭 타입 소거로 인해 T의 구체적인 타입 정보는 컴파일 타임 이후에 존재하지 않기 때문에, new T[]로 배열을 생성하면 배열의 타입 정보를 명확하게 유지할 수 없습니다.

```java
public class Box<T> {
    private T[] items;

    public Box() {
        items = new T[10];  // 컴파일 오류 발생!
    }
}
```
이 코드에서 컴파일러는 T가 어떤 타입인지 알지 못합니다. T는 제네릭 타입으로, T가 String, Integer, CustomClass 등 어떤 구체적인 타입이 될지 컴파일 타임에만 알 수 있습니다. 하지만 컴파일 후에는 이 타입 정보가 소거됩니다. 따라서 컴파일러는 이 시점에 new T[10]이 실제로 어떤 타입의 배열을 생성해야 하는지 알 수 없으므로, 컴파일 오류가 발생합니다.

이 문제를 해결하기 위해, 런타임에 배열의 타입을 명시적으로 지정할 수 있는 방법이 필요합니다. 여기서 java.lang.reflect.Array.newInstance가 사용됩니다. 이 메서드는 런타임에 실제 타입 정보를 전달받아 배열을 생성할 수 있습니다.

```java
import java.lang.reflect.Array;

public class Box<T> {
    private T[] items;

    public Box(Class<T> clazz) {
        items = (T[]) Array.newInstance(clazz, 10);
    }
}
```
여기서 Array.newInstance(clazz, 10)는 clazz가 나타내는 실제 타입의 배열을 생성합니다. 예를 들어, clazz가 String.class라면, String[] 배열이 생성됩니다.

### 비유로 이해하기

**제네릭 타입 소거**는 일종의 **포장지**라고 생각할 수 있어요. 우리가 어떤 선물을 줄 때, 선물을 포장지로 감싸죠? 이때 포장지의 겉모습만 보고는 안에 뭐가 들었는지 알 수 없어요. 마치 **제네릭 타입**이 뭐든 될 수 있지만, 정확히 뭔지 알 수 없는 상태와 같아요.

이제, 자바에서 `T`라는 제네릭 타입은 이 포장지처럼 어떤 "내용물"을 감싸는 역할을 해요. 컴파일(코드를 실행 가능한 상태로 만드는 과정) 중에는 이 포장지 안에 뭐가 들었는지 알아요. 그런데, **컴파일이 끝나면** 포장지는 사라져요(이걸 **타입 소거**라고 해요). 그래서 컴퓨터는 이제 **안에 뭐가 있었는지 모르고**, 단지 "뭔가 있긴 했지"라고만 기억해요.

### 왜 문제가 되나요?

자, 이제 우리가 컴퓨터한테 "안에 뭐가 있었는지 모르겠지만, 그걸로 새로운 상자를 만들어줘"라고 요청한다고 생각해봐요.
컴퓨터는 포장지가 사라진 상태에서, 즉 **내용물(실제 타입)을 모르는 상태**에서 새로운 상자를 만들 수 없어요.
"뭘로 상자를 만들어야 하지?"라고 고민하게 되죠. 그게 바로 `new T[]`로 배열을 만들 수 없는 이유예요.
컴퓨터는 `T`가 정확히 뭔지 모르기 때문에, 상자를 어떻게 만들어야 할지 모르는 거예요.

### 해결책

그래서 자바에서는 "**안에 뭐가 들어있는지 구체적으로 알려줘!**" 라고 말하는 방법이 필요해요.
그게 바로 `Array.newInstance`라는 방법이에요.

```java
items = (T[]) Array.newInstance(clazz, 10);
```

이 방법은 "포장지 안에 **뭐가 들어있었는지 정확히 알려줄게(예: `String.class` 같은 실제 타입)**. 이제 그걸로 새로운 상자를 만들어줘!"라고 말하는 거예요. 컴퓨터는 이제 "아! 그거였구나!" 하고, 정확히 어떤 상자를 만들어야 할지 알게 되는 거죠.

Array.newInstance: Array.newInstance(clazz, size)는 정확한 타입의 배열을 만들어주는 방법이에요. 여기서 clazz는 상자에 넣을 물건의 정확한 타입을 알려주는 포장지 역할을 해요. 예를 들어, Integer.class는 Integer 타입의 배열을 만들겠다고 알려줘요.

Array.newInstance 메서드는 런타임에 배열을 생성하고, 이 배열의 타입은 Object[]로 반환될 수 있어요. 왜냐하면, 제네릭 타입 T는 런타임에 구체적인 타입이 제거되기 때문에 컴퓨터는 정확한 타입 정보를 알 수 없어요.

### 요약

- **제네릭 타입 소거**: 포장지 안의 내용을 모르기 때문에 `new T[]`로 배열을 만들 수 없음.
- **Array.newInstance**: 컴퓨터에게 "포장지 안에 뭐가 있었는지 정확히 알려주는" 방법.

이제 컴퓨터는 안에 뭘 넣어야 할지 확실히 알고, 올바른 상자를 만들어줄 수 있어요!


## 왜 굳이 T[] 로 캐스팅을?
Array.newInstance(clazz, 10)를 사용하면 실제 타입의 배열을 생성할 수 있습니다.
하지만 이 메서드가 반환하는 배열의 타입은 런타임에서의 구체적인 타입 배열입니다.
반환 타입은 Object[] 타입으로 보일 수 있기 때문에, 이를 제네릭 타입 T[]로 사용할 수 있도록 변환이 필요합니다.

Array.newInstance 메서드는 Object[] 타입으로 반환되므로, 제네릭 타입 T[]으로 캐스팅합니다. 

### 쉽게 이해하기
- 상자 만들기: Array.newInstance는 정확한 타입의 상자를 만들기 위한 정보를 제공합니다.
  예를 들어, Integer 타입의 상자를 만들기 위해 필요한 정보가 담겨 있어요.
- 포장지 맞추기: 포장지와 상자의 타입이 맞아야 하기 때문에, 만든 상자를 정확한 타입으로 캐스팅합니다. 이것은 상자가 포장지와 정확히 맞게 만들어졌다는 것을 보장해줘요.


