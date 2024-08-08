# generic
> 제네릭은 형변환에 생길 수 있는 문제점을 컴파일 할 때 정정할 수 있도록 만들어졌다.
## 클래스 선언
![img](https://blog.kakaocdn.net/dn/cKJc01/btqv0yfQWxl/xyKRLszrlst7OlNRruz620/img.png)
- 클래스에 선언할 때는 `className<T>` 와 같이 선언하는데 이 괄호 안에는 실제 존재하는 클래스를 사용해도 되고 존재 하지 않는 것을 사용해도 된다.
- 꺽쇠 안에 선언된 이름은 클래스 안에서 하나의 타입처럼 사용된다. 가상의 타입 이름이라고 생각하자.
## 메소드 선언
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPKxdy%2FbtqG9A6wj3r%2F45am1mQxCFFbdUUvUCrh00%2Fimg.png)
- 메소드에서 제네릭 타입을 명시적으로 지정하기 애매할 경우에는 <> 안에 ? 라는 와일드 카드를 넣을 수 있다.
  - 단 이 경우 메소드내부는 해당 타입을 모르기 때문에 Object 로 처리 해야 한다.
- 와일드카드는 메개변수에만 사용하는 것이 좋다. 어떤 객체를 wildcard 로 선언하고, 그 객체의 값을 가져올 순 있지만 와일드 카드로 객체를 선언했을때 특정 타입으로 값을 지정하는 것이 불가능해진다.
- Bounded wild card 를 통해 어떤 클래스의 상속을 받은 특정 타입만 사용 가능하다는 것이 표현 가능하다.
  - `<? extends String>` 과 같이 사용할 수 있다.
- 두개 이상의 제네릭이 필요할 경우 <T,S>, <T, S extends String> 과 같이 사용할 수 있다.