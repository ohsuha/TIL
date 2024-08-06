# annotaion
- 컴파일러에게 정보를 알려주거나 실행시 별도의 처리가 필요할 때 사용한다.
- 클래스, 변수, 메소드 등에 모두 사용 가능하다.
- 상속할 수 없다.

### @Override
부모로부터 오버라이딩한 메소드임을 나타내기 위해 사용한다.

### @Deprecated
하위 호환성을 위해 코드를 지우지는 못했지만 더이상 사용하지 않음을 나타내기 위해 사용한다. 

### @SuppressWarnings
빌드시 발생하는 경고에 대해 인지하고 있으며 경고를 띄우지 말라는 의미로 사용된다.

## 어노테이션 선언을 위한 메타 어노테이션
```commandline
@Documented
@Inherited
@Target({ElementType.Type})
@Retention(RetentionPolicy.SOURCE)
public @interface AnnotaionName {}
```
### @interface
어노테이션 선언을 위해서 사용된다.
### @Target
어떤 것에 사용되는 어노테이션인지를 나타낸다 (메소드, 클래스, 변수, 빈...)
### @Retention
어노테이션 정보가 얼마나 유지되는지에 대한 정보
- SOURCE : 컴파일시 사라진다.
- CLASS : 컴파일러가 참조하지만 JVM은 참조하지 않는다.
- RUNTIME : JVM이 참조한다. 
### @Document
어노테이션에 대한 정보가 JavaDocs 에 있다
### @Inherit
상속받은 자식 클래스에서 부모 클래스의 어노테이션 사용 가능 여부
