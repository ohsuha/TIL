# Object.class
>모든 클래스의 최상위 부모 클래스, java.lang 에 선언되어있다.

Object 클래스에 있는 메소드들을 통해 클래스의 기본적인 행동을 정의할 수 있기 때문이다. 클래스라면 이 정도의 메소드는 정의되어 있어야 하고, 처리해 주어야 한다는 것을 
정의하는 작업이 필요하기 때문에 Object 클래스를 상속 받았다고 생각하면 된다.
 

## 객체 처리를 위한 메서드
### toString()
``ClassName.ToString@1540e19d`` 와 같은 형식으로 객체를 문자열로 표현하는 값을 리턴한다.<br>
객체에 ``+`` 연산을 하거나 ``System.out.print()`` 에 들어가면 자동으로 호출된다.

### equals()
- 현재 객체와 매개변수로 넘겨받은 객체가 같은 객체인지를 비교한다.
- 두 객체의 같고 다름을 참조변수의 값으로 판단한다. 
- ``equals()`` 메소드를 ``overriding`` 하지않으면 ``equals()`` 메소드에서는 ``hashCode()`` 값을 비교한다. <br>
- 참조형일 경우 주소값을 비교한다. 따라서 내부 변수가 같은지를 체크하려면 ``overriding`` 해야한다. 이때 다음의 5가지 조건을 만족시켜야한다.
  - 재귀 : null 이 아닌 x 라는 객체의 `x.equals(x)` 는 항상 ``true`` 여야 한다.
  - 대칭 : null 이 아닌 x 라는 객체와 y 라는 객체가 있을때 `y.equals(x)` 가 `true` 라면 `x.equals(y)`도 `true`여야 한다.
  - 타동적 : null 이 아닌 x,y,z 가 있을때 `x.equals(y)` 가 `true`, `y.equals(z)` 가 `true` 라면 `x.equals(z)` 는 반드시 `true`여야만 한다.
  - 일관 : null 이 아닌 x 라는 객체와 y 라는 객체가 있을때 객체가 변경되지 않은 상황에서는 몇 번을 호출하더라도 `x.equals(y)` 는 항상 `true` 이거나 `false` 여야 한다.
  - `null` 과의 비교 : `null` 이 아닌 x라는 객체의 `x.equals(null)` 결과는 항상 `false` 여야한다.

``String.class > equals overriding`` 예시
```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    return (anObject instanceof String aString)
            && (!COMPACT_STRINGS || this.coder == aString.coder)
            && StringLatin1.equals(value, aString.value);
}

//StringLatin1
public static boolean equals(byte[] value, byte[] other) {
    if (value.length == other.length) {
        for (int i = 0; i < value.length; i++) {
            if (value[i] != other[i]) {
                return false;
            }
        }
        return true;
    }
    return false;
}
```
### hashCode()
객체에 대한 해시코드, 16진수로 제공되는 객체의 메모리 주소를 리턴한다.<br>
어떤 두개의 객체가 서로 동일하다면 hashCode() 값은 무조건 동일해야한다.<br>
따라서 equals() 메소드를 override 한다면 hashCode()도 override 해서 동일한 결과가 나오도록 만들어야만 한다.<br>

#### hashCode() overriding 할 때에의 조건
- 자바 애플리케이션이 수행되는 동안에 어떤 객체에 대해서 이 메소드가 호출될 떄에는 항상 동일한 int 값을 리턴해 주어야 한다. 하지만 자바를 실행할 떄마다 같은 값이어야 할 필요는 없다.
- 어떤 두개의 객체에 대하여 equals()메소드를 사용하여 비교한 결과가 true 일 경우에, 두 객체의 hashCode() 메소드를 호출하면 동일한 int 값을 리턴해야한다.
- 두 객체를 equals() 메소드를 사용하여 비교한 결과 false 를 리턴했다고 해서, hashCode() 메소드를 호출한 int 값이 무조건 달라야할 필요는 없다. 하지만 이 경우 서로 다른 int 값을 제공하면 hashtable의 성능을 향상 시키는 데 도움이 된다.

### clone()
- 자신을 복제하여 새로운 인스턴스를 생성하는 일을 한다. 단순히 인스턴수 변수의 값만 복사하기 때문에 참조 타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이뤄지지 않는다.
- 복제할 클래스가 Cloneable 인터페이스를 구현해야한다.
#### 공변 반환 타입
JDK1.5 부터 추가되었는데 오버라이딩할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용하는 것이다. 
예전에는 오버라이딩할 때 조상에 선언된 메서드의 반환 타입을 그대로 사용해야 했다.
```java
point copy  = (Point)original.clone();
- 공변 반환 타입 -> point copy = original.clone();
```
#### 얕은 복사와 깊은 복사
- clone()은 객체에 저장된 값을 그대로 복제할 뿐, 객체가 참조하고 있는 객체까지 복제하지는 않는다.
- 원본과 복제본이 같은 객체를 공유하므로 완전한 복제라고 보기 어렵다.
- 얕은 복사 : 원본을 변경하면 복사본도 영향을 받는다.
- 깊은 복사 : 원본이 참조하고 있는 객체까지 복제하는 것, 원본과 복사본이 서로 다른 객체를 참조하기 때문에 원본의 변경이 복사본에 영향을 미치지 않는다.


### getClass()
- 자신이 속한 클래스의 Class 객체를 반환하는 메서드이다.
- 클래스당 1개만 존재한다. 그리고 클래스파일이 클래스 로더에 의해서 메모리에 올라갈 떄, 자동으로 생성된다. 
- 파일 형태로 저장되어 있는 클래스를 읽어서 Class 클래스에 정의된 형식으로 변환하는 것이다.

### finalize()
가비지 컬렉터가 객체를 삭제할때 호출한다.

## 스레드 처리를 위한 메서드
### notify(), notifyAll()
이 객체의 모니터에 대기하고 있는 단일 / 모든 스레드를 깨운다.

## 가비지 컬렉터에 의해 호출되는 메서드
### finalize()
객체가 소멸될 때 가비지 컬렉터에 의해 자동적으로 호출된다.

### wait()
다른 스레드가 현재 객체에 대한 notify() 메소드를 호출할 떄까지 현재 스레드가 대기하고 있도록한다.