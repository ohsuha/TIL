# 롬복 실행 원리
사용 이유 : 보일러 플레이트 코드들을 어노테이션을 통해 작성하지 않아도 되도록 해준다.

컴파일 시 자동으로 코드 생성: 롬복은 자바 코드를 컴파일할 때, 우리가 적은 어노테이션을 보고 필요한 메서드들을 자동으로 생성해줘. 즉, 코딩할 때는 안 보이지만, 컴파일된 클래스 파일에는 롬복이 만들어준 코드가 포함돼 있어.

바이트코드 수정: 자바 컴파일러가 코드를 컴파일할 때, 롬복이 해당 어노테이션을 보고 코드에 추가할 메서드들을 바이트코드로 변환해서 넣어줘. 이 과정은 자바의 애노테이션 프로세서를 통해 이루어져.

1. javac는 소스파일을 파싱하여 AST트리를 만든다. 
2. Lombok은 AnnotaionProcessor에 따라 AST 트리를 동적으로 수정하고 새 노드(소스코드)를 추가하고 마지막으로 바이트 코드를 분석 및 생성한다.
(컴파일 과정에서 생성된 Syntax Tree는 com.sun.source.tree.*에서 public accesss를 제공한다.)
3. 최종적으로 javac는 Lombok Annotation Processor에 의해 수정된 AST를 기반으로 Byte Code를 생성한다.
   이제 좀 더 깊이 들어가서 롬복(Lombok)이 자바 코드에 어떻게 작용하는지 고등학생이 이해할 수 있도록 쉽게 설명해볼게!

### 자바 컴파일 과정 간단히 설명

우리가 자바 코드를 작성하고, **javac**라는 컴파일러를 사용해서 컴파일하면, **소스 코드**(우리가 작성한 자바 파일)가 컴퓨터가 이해할 수 있는 **바이트코드**로 변환돼. 이 바이트코드는 JVM(자바 가상 머신)이 실행할 수 있는 형태로 변하는 거지.

자바 코드를 컴파일하는 과정은 몇 단계로 나뉘어:
1. **파싱(Parsing)**: 자바 컴파일러가 **소스 파일을 읽고 해석**해. 이 과정에서 코드의 구조가 **추상 구문 트리(AST)**라는 형태로 변환돼. 이 트리는 컴파일러가 코드를 이해하는 방식이야. 예를 들어, 클래스, 메서드, 변수 등을 나무처럼 분리해서 구조화하는 거지.

2. **바이트코드 생성**: 컴파일러는 이 구조화된 트리(AST)를 분석한 후 **바이트코드**로 변환해.

### Lombok이 컴파일 과정에 끼어드는 방식

이제 롬복이 이 과정에서 어떻게 동작하는지 알아보자.

1. **AST 트리 파싱**: 자바 컴파일러가 코드를 읽고, 구조를 만들 때, 롬복은 **자바의 애노테이션 프로세서(Annotation Processor)**로 동작해. 롬복은 이 단계에서 **`@Getter`, `@Setter` 같은 어노테이션**을 보고, 해당 코드에 어떤 변경이 필요한지 감지해.

2. **AST 트리 수정**: 롬복은 애노테이션 프로세서를 통해 **AST 트리를 동적으로 수정**해. 쉽게 말하면, 우리가 `@Getter` 같은 어노테이션을 붙였을 때, **컴파일 단계에서 실제로 `getName()` 같은 메서드를 추가해주는 것**이지. 이 메서드를 직접 쓰지 않았지만, 컴파일러는 마치 우리가 쓴 것처럼 생각하게 만들어 주는 거야.

    - 이 과정에서 롬복은 **새로운 소스 코드(메서드)**를 만들어서 트리에 넣어. 이런 식으로, 컴파일 전에 이미 우리가 필요한 코드가 다 들어가게 돼.

3. **최종 바이트코드 생성**: 컴파일러는 롬복에 의해 수정된 **최종 AST 트리**를 기반으로 **바이트코드**를 생성해. 이 바이트코드는 JVM에서 실행될 준비가 완료된 거지.

### 비유로 설명

- **파싱과 AST**: 컴파일러가 자바 코드를 읽고 나무처럼 구조를 세우는 걸 생각해 봐. 예를 들어, 우리가 쓴 코드가 **건물의 설계도**라고 생각하자. 컴파일러는 이 설계도를 바탕으로 건물을 세우기 전에, 각각의 방, 문, 창문 등을 **세부적으로 나눈 그림**을 만들어. 이게 바로 **AST 트리**야.

- **롬복의 역할**: 롬복은 이 설계도를 보면서 "어? 여기에 문이 하나 더 필요하네!" 하고 **자동으로 문을 추가하는 친구**라고 생각하면 돼. 우리가 직접 문을 그리지 않아도, 설계도를 분석해서 필요한 문을 롬복이 추가해주는 거야. 우리가 `@Getter` 같은 어노테이션을 달면, 컴파일러가 설계도를 다 만들기 전에 롬복이 "여기 get 메서드를 추가해야지!" 하고 알아서 코드를 추가해 주는 거지.

- **최종 컴파일**: 설계도가 다 완성되고 나면, 컴파일러는 이 설계도(수정된 AST 트리)를 바탕으로 **최종 건물**(바이트코드)을 완성해. 이 최종 건물은 컴퓨터가 이해할 수 있는 형태로 변환된 것이지.

### 핵심 요약
1. **javac(자바 컴파일러)**는 우리가 쓴 자바 코드를 읽고, **AST 트리**라는 구조를 만든다.
2. **롬복**은 자바 컴파일러가 이 트리를 만드는 과정에서 **어노테이션을 보고 코드를 동적으로 추가**한다.
3. 마지막으로 컴파일러는 롬복에 의해 수정된 **AST 트리**를 바탕으로 **바이트코드**를 생성하여, 실행 가능한 프로그램으로 변환한다.

이 과정 덕분에 우리는 반복적으로 코드를 작성할 필요 없이, **롬복이 자동으로 필요한 부분을 채워**주게 되는 거야!

# 패키지 구조에 대해서 고민해 본 적이 있는지? (by layer vs by domain)

# Facade 패턴은 왜 사용 하는지?

# 빌더 패턴을 사용하는 이유?
1. setter 를 만들지 않기 위해서 : setter 를 통해서 파라미터의 값이 의도치 않게 변경되는것을 막고자 -> 객체의 불변성을 유지하기 위해서
2. 생성자를 만들때보다 가독성이 좋아서 : 변수가 많은 객체일경우 생성자를 순서대로 넣어줘야하는데 이것보다 가독성이 좋아서
3. 불필요한 파라미터를 제외하고 객체를 생성하고 싶어서, 그때마다 생성자를 만드는것보다 편리하기 떄문에