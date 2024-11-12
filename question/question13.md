# redis, get, hash table, skip list 관계 제대로 이해하기
- 레디스에서 키를 관리하는 데이터 구조는 해시 테이블이다. 해시 테이블은 키 관리 뿐만아니라 set과 hashes 에서도 사용된다.
- 레디스는 메모리 공간 절약을 위해 ZIP List 를 사용한다. 데이터의 형태, 길이에 맞게 최대한 메모리를 절약할 수 있는 구조를 사용한다.
- Sorted Set은 내부적으로 두 가지 데이터 구조로 저장된다.   레디스가 데이터 구조를 선택하는데, 두 가지 조건이 있다.
- 하나는 멤버 수가 128개 까지는 zip list라는 데이터 구조에 저장되고, 129개부터는 데이터 구조를 변환에서 skip list에 저장된다.
- 원래의 스킵리스트는 수정/삭제에 시간이 오래 걸리기 때문에 레디스에서는 원래 스킵 리스트에 몇 가지 수정을 해서 구현했다.
  - 같은 스코어가 반복되는 것을 허용한다.
  - 그래서 LEX 명령을 사용할 수 있게 했다.
  - LEX 명령을 사용하려면 스코어는 모두 같아야 한다.
  - 값(value)는 달라야 한다. 
  - 위와 같이 LEX를 구현하기 위해서 스코어가 같으면 값(value)을 비교한다.  
  - 역 탐색을 위해서 이전 노드를 가리키는 포인터(back pointer)를 둔다.
  - 최대 레벨을 32로 했다.
  - 스킵 리스트 자체를 저장하는 구조체에 최대 레벨과, 노드 수(length), 헤더 노드, 마지막(tail) 노드의 포인터를 가지고 있다.
- Redis는 내부적으로 각 자료형의 크기나 요소 수에 따라 hashTable, skipList, zipList 중 하나를 자동으로 선택하여 데이터를 저장합니다.
- GET 명령으로 값을 요청할 때, Redis는 이 자료구조들 중에서 적절한 방식으로 데이터를 조회하여 반환합니다. 
  - Hash 타입: 요소가 적을 때는 zipList를, 많을 때는 hashTable을 사용합니다. O(1)
    - 키를 기준으로 데이터에 직접 접근할 수 있기 때문에, 평균적으로 O(1)의 시간 복잡도로 특정 키의 데이터를 빠르게 조회할 수 있습니다.
  - skipList : 대부분 skipList를 사용합니다. O(logN)
    -  정렬된 데이터의 범위 조회와 특정 요소 검색에 효율적인 자료구조로, O(log N) 시간 복잡도로 특정 값에 접근할 수 있습니다. 이는 정렬된 집합(Sorted Set)에서 주로 사용
  - List 타입: 데이터가 작으면 zipList O(N)
    - 연속된 메모리 블록에 데이터가 압축되어 저장되며, 각 항목은 순차적으로 저장됩니다. 따라서 데이터의 크기가 작고 항목 수가 적은 경우 메모리를 매우 효율적으로 사용하지만, 특정 값을 찾으려면 최대 O(N)의 시간이 걸릴 수 있습니다.
- 따라서 GET으로 데이터를 가져올 때 Redis는 저장된 자료형의 크기와 형태에 따라 각각의 자료구조에서 데이터를 조회합니다.

참고 : http://redisgate.kr/redis/configuration/internal_skiplist.php
# redis를 통해서 lock을 구현하는 방법에 대해 개념적으로 찾아보기
여러 분산된 서버가 존재할 때, 여러 서버에서 공유 자원에 접근하여 발생하는 Race Condition 자체를 없앤다.

분산 락을 구현하기 위해 락에 대한 정보를 ‘어딘가’에 공통적으로 보관하고 있어야 한다.
그리고 분산 환경에서 여러대의 서버들은 공통된 ‘어딘가’를 바라보며, 자신이 임계 영역(critical section)에 접근할 수 있는지 확인한다.
이렇게 분산 환경에서 원자성(atomic)을 보장할 수 있게 된다.
그리고 그 ‘어딘가’로 활용되는 기술은 MySQL의 네임드 락, Redis, Zookeeper 등이 있다.

대기가 없는 경우 락 획득 후, true를 반환합니다.
이미 누군가 락을 획득한 경우, pub/sub에서 메시지가 올 때까지 대기하다가, 락이 해제되었다고 메시지가 오면 대기를 해제하고 락 획득을 시도합니다. 만약 락 획득에 실패하면 락 해제 메시지를 기다리고 타임아웃까지 기다립니다.
타임아웃이 지나며 최종적으로 false를 반환하고 락 획득을 실패했다고 합니다.

- tryLock()로 락 획득을 시도
- 락을 획득했으면 true 리턴
- 락 획득 실패시 waitingTime 확인 후 시간이 남았으면 pubsub 구독 후 다시 락 획득이 가능하면 다시 시도
- 위의 과정을 waitingTime이 다 될 때까지 무한반복

- 획득하고자 하는 이름의 락을 정의하고 유효 시간까지 락 획득을 시도하게 된다
- 만약 유효 시간이 지나면 락 획득은 실패하게 된다 
- 단 여기서 주의할 점은 unlock할 경우에, 해당 세션에서 잠금을 생성한 락인지 고유 번호등으로 확인한다

- Lettuce - Spin Lock으로 지속적으로 Redis 서버에 요청을 보내서 Lock 여부 확인 (스핀락)
- Redision : RedLock 알고리즘을 사용해서 Lock 획득-해제를 pub-sub 구조로 수행
- 지속적으로 Lock을 획득하기 위해 Redis 서버로 요청을 보내는 것이 아니라, pub-sub 구조로 Lock을 해제하는 쪽에서 Lock 해제 메시지를 발행하고, Lock을 얻기 위해 대기하는 쪽에서 해당 메시지를 받아서 Lock을 획득하기 때문에 부하가 훨씬 덜합니다.

https://velog.io/@penrose_15/Redisson-tryLock-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95

# tree에서 검색이 log n 만큼 걸리는 이유
- 찾고자 하는 값이 루트 노드보다 작다면 왼쪽 자식 노드로 이동하고, 크다면 오른쪽 자식 노드로 이동합니다. 이렇게 함으로써 검색 범위가 절반으로 줄어들게 됩니다.
-  트리의 높이는 검색에 필요한 최대 단계 수에 해당합니다. 따라서 균형 잡힌 트리에서 검색의 시간 복잡도는 O(logN)이 됩니다.

# 스프링에서 DI를 통해서 해결하고 싶은 문제는 뭘지 생각해보기
- 클라이언트 코드에서 직접 다른 클래스의 객체를 생성하면 특정 구현체에 종속되어 해당 클래스의 수정이나 교체가 어려워진다.
- DI를 통해 외부에ㅓㅅ 주입받게 하면 클래스가 특정 구현체에 종속되지 않게 된다. 인터페이스를 통해 여러 구현체를 사용할 수 있어 코드 재사용과 확장성을 높인다.
- 클래스 내에서 객체를 직접 생성하면 해당 클래스는 외부 라이브러리나 다른 클래스의 변화에 민감해지며 의존하는 객체가 많을 수록 코드가 복잡해진다.
- DI를 통해 외부에서 객체를 주입 받으면 변경이 필요할 때 주입되는 객체의 설정만 수정하면 되므로 클래스간 결합도가 낮아진다.
- 이 관리를 스프링 컨테이너가 함
# CGLIB vs JDK Dynamic Proxy
- 프록시 객체를 생성하여 메서드를 동적으로 호출하기 위한 방법이다. AOP같은 기능을 구현하기 위해 이 기법을 사용한다.
## JDK Dynamic proxy
- java.lang.reflect.Proxy 를 사용해서 인터페이스 기반의 프록세를 생성한다.
- 프록시 생성을 위해서는 대상 클래스가 반드시 인터페이스를 구현해야한다.
- 런타임에 프록시 개체를 생성하고 InvocationHandler 인터페이스의 invoke 메서드를 통해 메서드 호출을 가로 챈다.
- 인터페이스에 정의된 메서드만 호출할 수 있다.
- 자바 표준 라이브러리 사용
- 클래스 자체에 의존하는 메서드는 프록시로 감싸지 못한다.
## CGLIB
- 구체 클래스의 프록시를 생성할 수 있또록 지원하는 라이브러리
- 바이트코드 생성 라이브러리인 ASM 을 사용해 클래스를 상속받아 프록시 클래스를 생성한다.
- 대상 클래스의 서브 클래스를 만들어 메서드를 오버라이딩하여 호출을 가로챈다. 상속을 통해 동작하므로 final 메서드는 오버라이드할 수 없고 프록시 할 수 없다.
- 인터페이스가 없는 구체 클래스에서 프록시를 사용할 수 있다.
- 스프링에서는 인터페이스가 없는 빈에 대해서 프록시 지원을 위해 CGLIB 을 기본으로 제공
- 추가 라이브러리가 필요하다.

# 생성자 주입을 선호하는 이유 고민해보기
1. 불변성 보장 : 생성자 주입을 사용하면 주입된 의존성을 final 로 선언할 수 있어서 의도하지 않은 변경이나 의존성의 교체가 발생하지 않는다.
2. 필수 의존성 설정 강제 : 생성자 주입은 객체 생성시 반드시 의존성을 주입해야하므로 필요한 모든 의존성이 없으면 객체가 생성되지 않아 의존선 누락 없이 런타임 에러를 예방할 수 있다.
3. 순환 참조 방지 : 두 클래스가 서로를 생성자 주입하려 할때 발생하는 순환 참조 문제를 컴파일 시점에 감지해줘서 순환참조가 발생하지 않도록 설계할 수 있다.
4. 테스트 용이성 : 테스트시 Mock 객체를 주입하거나 테스트 대체 객체를 사용할 수 있어 테스트 코드 작성이 쉬워지고 의존성이 명확해진다.

# 코드 커버리지

# CI/CD 개념을 이해하기
# github action

# docker 개념 학습하기 시작하기
https://pyrasis.com/docker.html