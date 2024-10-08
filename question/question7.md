# hashCode는 어디에 쓰이는지?
해싱 기법에 사용되는 해시함수를 구현한 것이다.
해시맵이나 해시셋같은 클래스에 저장할 객체라면 반드시 해시코드 메서드를 오버라이딩해야한다.
스트링 클래스는 문자열의 내용이 같으면 동일한 해시코드를 반환하도록 해시코드 메서드가 오버라이딩되어있기 때문에,
문자열의 내용이 같은 스트링 객체에 대해서 해시코드를 호출하면 항상 동일한 해시코드 값을 얻는다.
반면에 System.identityHashCode(Object x) 는 오브젝트 클래스의 해시코드 메서드처럼 객체의 주소값으로 해시코드를 생성하기 때문에 모든 객체에 대해
항상 다른 해시코드값을 반환할 것을 보장한다. 그래서 스트링 객체의 문자열이 같아도 서로 다른 객체라는 것을 알 수 있다.

해시맵과 같이 해싱을 구현한 컬렉션 클래스에서는 오브젝트 클래스에 정의된 해시코드를 해시함수로 사용한다.
오브젝트 클래스에 정의된 해시코드는 객체의 주소를 이용하는 알고리즘으로 해시코드를 만들어 내기때문에 모든 객체에 대해 해시코드를 호출한 결과가 서로 유일한 방법이다.

스트링 클래스의 경우 해시코드메서드를 오버라이딩해서 문자열의 내용으로 해시코드를만들어 낸다.
그래서 서로 다른 스트링객체일지라도 같은 내용의 문자열을 가졌다면 해시코드 호출시 동일한 해시코드를 얻는다.

서로다른 두 객체에 대해 equals()로 비교한 결과가 True인 동시에 hashCode()의 반환값이 같아야 같은 객체로 인식한다.
해시맵에서도 같은 방법으로 객체를 구별하며, 이미 존재하는 키에 대한 값을 저장하면 기존의 값을 새로운 값으로 덮어쓴다.
그래서 새로운 클래스를 정의할 때 이퀄스를 재정의 오버라이딩해야한다면 해시코드도 같이 재정의해서 이퀄스의 결과가 true인
두 객체의 해시코드 결과값이 항상 같도록 해줘야한다. 그렇지 않으면 hashMAP과 같이 해싱을 구현한 컬렉션 클래스에서는
이퀄의 호출결과가 트루지만 해시코드가 다른 두 객체를 서로 다른 것으로 인식하고 따로 저장할 것이다.

참고로 이퀄스로 비교한 결과가 false이고 해시코드 가 같은 경우에는 같은 링크드 리스트에 저장된 서로 다른 두 데이터가 된다.

HashMap과 HashSet 같은 해시 기반 자료 구조는 먼저 객체의 hashCode()를 사용해 객체를 특정 버킷에 할당합니다. 그런 다음, 동일한 해시 값에 속하는 객체들에 대해 equals() 메서드로 비교하여 실제로 동일한지 확인합니다.
따라서 equals()가 같은 두 객체는 항상 같은 hashCode() 값을 반환해야 합니다. 그렇지 않으면, 동일한 객체임에도 불구하고 다른 버킷에 저장되거나, 찾지 못하는 문제가 발생할 수 있습니다.

# 쓰레드, 프로세스 메모리 관리 관점에서 다시 찾아보기
| **특성**               | **프로세스 (Process)**                               | **쓰레드 (Thread)**                                 |
|------------------------|------------------------------------------------------|-----------------------------------------------------|
| **메모리 공간**        | 독립적인 메모리 공간 (가상 메모리)                   | 프로세스 내에서 메모리를 공유                       |
| **메모리 구조**        | 코드, 데이터, 힙, 스택 영역이 각각 독립적             | 스택은 개별적, 코드, 데이터, 힙 영역은 공유         |
| **메모리 접근**        | 다른 프로세스의 메모리 접근 불가 (IPC 필요)           | 같은 프로세스 내에서 메모리 자원 공유 가능          |
| **문맥 전환 비용**     | 프로세스 전환 시 비용이 큼 (전체 메모리 맵 전환)       | 쓰레드 전환 비용이 적음 (레지스터와 스택만 전환)   |
| **안전성**             | 프로세스 간 메모리 격리로 안정성이 높음               | 메모리를 공유하기 때문에 동기화 문제 발생 가능      |
| **주요 동기화 문제**   | 다른 프로세스와 동기화 문제는 적음                    | 자원 공유로 인해 경쟁 상태 발생 가능, 동기화 필요   |

# 컨텍스 스위칭 관점에서 어떤 차이가 있는지
## 프로세스가 컨텍스트 스위칭에서 비용이 큰 이유
- 가상 메모리 주소 공간 전환: 프로세스마다 고유한 가상 메모리 공간을 관리해야 하므로 주소 공간 변경 작업이 필요합니다.
- 캐시와 TLB 무효화: 캐시와 TLB의 플러시가 발생하여 새로운 프로세스에 맞게 다시 로드해야 합니다.
- 자원 관리 비용: 파일 디스크립터, 소켓 등 시스템 자원의 상태를 관리해야 합니다.
- 문맥 전환 정보 처리: CPU 레지스터, 스택, 주소 공간 관련 정보를 모두 저장하고 복원하는 추가적인 비용이 발생합니다.
## 스레드의 컨텍스트 스위칭
쓰레드의 컨텍스트 스위칭은 같은 프로세스 내에서 실행되는 여러 쓰레드들 간에 CPU 자원을 할당하는 과정입니다.
쓰레드는 프로세스의 주소 공간을 공유하기 때문에, 프로세스 간 컨텍스트 스위칭보다 더 빠르게 이루어집니다.
그러나 여전히 CPU 레지스터와 스택 같은 쓰레드의 **문맥(context)** 을 저장하고 복원하는 작업이 필요합니다.
여러 쓰레드가 메모리를 공유하기 때문에, 동기화 문제로 인한 성능 저하가 발생할 수 있습니다.

# 싱글 스레드, 멀티 스레드로 처리하는 것의 장단점
- 싱글스레드
    - 장점 : 구현이 쉽다. 동기화 문제가 없다. 여러 스레드를 생성하지 않아 메모리가 적게 든다.
    - 단점 : 하나의 작업만 처리할 수 있어 병렬 처리가 불가능하다. 하나의 작업이 오래 걸리면 그동안 다른 작업을 처리할 수 없다(응답성 저하), 하나의 코어만 사용하므로 CPU 리소스를 비효율적으로 사용한다.
- 멀티스레드
    - 장점 : 여러 작업을 동시에 병렬처리가 가능. 긴 시간이 소요되는 작업을 백그라운드 스레드로 처리, 메인은 계속 사용자의 요청을 받을 수 있어 응답성이 유지된다. 같은 프로세스 내에서 여러 스레드가 메모리와 자원을 공유하므로 데이터를 쉽게 주고받을 수 있다.
    - 단점 : 구현이 어렵다. 동기화 문제가 일어난다(락, 세마포어 등의 기법을 사용해 해결), 각 스레드는 스택 메모리와 컨텍스트를 필요로 하므로 스레드를 많이 생성하면 그만큼 메모리를 더 사용하게된다. 또 컨텍스트 스위칭시 CPU가 스레드의 상태를 저장하고 복원하는 작업을 해야해서 스레드가 많아지면 성능이 저하될 수 있다. 동기화 과정에서 경합, 경쟁이 발생해 처리 속도가 느려질 수 있다.

| **항목**            | **싱글 스레드**                                     | **멀티 스레드**                                    |
|---------------------|----------------------------------------------------|---------------------------------------------------|
| **구현 난이도**      | 쉬움 (동기화 문제 없음)                              | 어려움 (동기화 및 경쟁 상태 관리 필요)              |
| **병렬 처리**        | 불가능 (하나의 작업만 순차 처리)                      | 가능 (여러 작업을 동시에 처리)                      |
| **응답성**           | 작업이 길어지면 응답성 저하                          | 작업을 분리하여 응답성 유지 가능                    |
| **CPU 활용도**       | 낮음 (CPU 코어를 하나만 사용)                        | 높음 (여러 CPU 코어를 사용할 수 있음)              |
| **메모리 사용**      | 적음 (단일 스레드로 메모리 효율적 사용)               | 높음 (여러 스레드 생성 및 스택 메모리 필요)         |
| **디버깅 및 유지보수** | 쉬움 (동기화 문제 없음)                              | 어려움 (비동기 실행으로 버그 추적 어려움)           |
| **컨텍스트 스위칭**  | 없음                                                | 있음 (스레드 간 전환에 따른 오버헤드 발생 가능)     |
| **동기화 문제**      | 없음                                                | 있음 (경쟁 상태, 교착 상태 문제 가능)               |



# 싱글 스레드, 멀티 스레드로 처리할 때 유리한 작업
- 싱글스레드
    - 단순하고 짧은 작업 : 실행시간이 짧은 작업. 간단한 계산
    - 동기적 흐름이 필요한 작업
    - 여러 스레드가 동시에 접근하면 문제가 생기므로 상태 유지가 필요한 작업
- 멀티스레드
    - I/O 바운드 작업 : 네트워크 요청, 파일 시스템 작업등에서 사용하면 스레드가 대기하는 동안 다른 스레드가 작업을 계속할 수 있다.
    - CPU 바운드 작업 : CPU의 계산능력을 최대한 활용해야 할 때, 여러 스레드가 코어를 활용하며 병렬로 작업을 처리하여 성능을 극대화 할 수 있다.
    - 비동기 처리 : 사용자의 요청을 동시에 처리해야하는 경우, 사용자가 이전 요청의 응답을 대기하지않고 사용해야 할 때

# 적당한 스레드 숫자
> 스레드풀의 적정 크기 = cpu 코어개수 * (1/한 요청당 CPU 사용시간)

# TCP에서 순서를 보장하는 것이 왜 존재하는지? 순서가 왜 틀어질 수 있는지
## 신뢰성을 유지하기 위해 왜 순서가 중요한가?
- 채팅, 비디오 스트리밍에서는 올바른 순서대로 재생되어야한다. 순서가 틀어지면 데이터의 의미가 달라진다.
- 상태기반의 애플리케이션에서는 이전 요청의 결과에 따라 다음 요청이 달라질 수 있다. 순서가 보장되지 않으면 상태가 이상하게 바뀔 수 있다.
- 수신 측에서 데이터를 순서대로 처리해야 오류를 올바르게 감지하고 수정할 수 있다. 순서가 바뀌면 오류가 발생했는지 확인하기 어렵다.
- 순서가 바뀌면 처리 순서가 지연되거나 잘못된 순서로 응답이 생성될 수 있다.
## 순서가 왜 틀어질 수 있지?
- 패킷이 여러 라우터와 스위치를 거치면서 전송되기 때문에 서로 다른 경로를 통해 목적지에 도착할 수 있다.
- 네트워크의 혼잡 등으로 인해 일부 패킷이 다른 패킷보다 늦게 도착할 수 있다.
- 여러 경로로 동시에 전송해서 속도를 높이는데 이때문에 패킷의 순서가 뒤바뀔 수 있다.
## 어떻게 이를 해결하지?
- 각 패킷에 시퀀스 번호를 부여하고 수신측에서 패킷을 시퀀스에 따라 올바른 순서로 재조합한다.

# private/public ip 개념
- public : 인터넷에서 직접 접근할 수 있는 주소 ISP 에 의해 할당되며 전 세계적으로 고유하다.
- private : 특정 네트워크 내에서만 유효한 주소, 공용 인터넷에서 직접 접근할 수 없으며 동일한 사설 IP가 여러 네트워크에서 사용될 수 있다. (가정용, 기업)

# 적은 ip로도 통신이 가능한지 찾아보기
1. IPv4 는 43억개이지만 IPv6은 거의 무한에 가까운 아이피 주소를 제공한다.
    - IPv4 : 32 비트, 6은 128비트
2. NAT는 하나의 공인 IP 주소를 사용해서 여러 장치가 인터넷에 접속할 수 있도록 해주는 기술. 내부에서는 사설 IP 주소를 쓰지만 외부 인터넷과 통신할때는 공유기의 공인 IP 주소를 사용한다. NAT는 사설 IP주소를 공인 IP주소로 변환해준다.
3. 동적 IP 할당 DHCP : 인터넷 서비스 제공업체는 인터넷에 접속할때마다 새로운 IP주소를 동적으로 할당한다. 인터넷을 사용하지 않으면 해당 IP를 다른 사용자에게 할당할 수 있다.

# readonly를 붙이면 성능이 좋아지는 이유?
JPA의 영속성 컨텍스트와 더티 체킹

## 더티 체킹
1. 영속성 컨텍스트는 엔티티 조회시 초기 상태에 대한 스냅샷을 저장한다.
2. 트랜잭션이 커밋 될때 초기 상태 정보를 가지는 스냅샷과 엔티티의 상태를 비교해 변경된 내용에 대해 update 쿼리를 생성해 쓰기 지연 저장소에 저장한다.
3. 일괄적으로 쓰기 지연 저장소에 저장된 뭐리들을 flush 하고 데이터 베이스의 트랜잭션을 커밋해서 엔티티의 수정을 진행한다

readOnly = true 를 설정하면 JPA의 세션 플러시 모드를 MANUAL로 설정한다 (MANUAL 모드는 사용자가 수동으로 flush 를 호출해야하만 호출됨)
트랜잭션 내에서 flush()를 호출하지 않으면 수정 내역에 대해 DB에 적용하지 않는다. 따라서 commit 시 자동으로 flush 되지 않아서 조회용으로 가져온 엔티티의 예상치 못한 수정을 방지할 수 있다.
**이를 설정하면 JPA는 트랜잭션 내에서 조회하는 엔티티가 조회용임을 인식하고 변경 감지를 위한 스냅샷을 따로 보관하지 않으므로 메모리가 절약되는 성능상 이점이 존재한다.**

## 트랜잭션 아예 안붙이면 되지 않아?
OSIV를 false 로 설정하면 영속성 컨텍스트는 트랜잭션의 범위를 벗어나는 순간 영속성 컨텍스트의 관리를 받지 않는 준영속 상태가 되어버린다.
즉 레이지 로딩의 동작이 불가능해진다. OSIV를 false 로 설정하고 @Transactional 어노테이션을 제거하면 LazyInitializationException 이 발생한다.

## 트랜잭션을 꼭 붙어야하나?
@Transactional 어노테이션을 붙이게 되면 해당 영역에서는 JPA의 스냅샷 유지, flush의 필요성, DB 커넥션을 오래 물고 있는 등의 관리적인 측면이 발생한다.
따라서, lazy-loading, replication과 같이 트랜잭션 범위 내에서 수행해야 되는 동작이 있는 경우에 대해서 적절히 @Transactional 어노테이션을 활용하는 것이 좋으며,
무분별하게 @Transactional 어노테이션을 사용하는 것은 위에서 언급했듯이 스냅샷 유지, flush의 필요 등 관리적/메모리적 측면에서 오히려 좋지 않을 수 있고, 커넥션을 오래 가지고 있어 커넥션 부족 등의 문제가 발생할 수 있다.

# 트랜잭션 격리 수준에 대해서 찾아보기
> 트랜잭션의 격리 수준(Isolation Level)이란 여러 트랜잭션이 동시에 처리될 때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 여부를 결정하는 것

모두 자동 커밋(AUTO COMMIT)이 false인 상태에서만 발생
- SERIALIZABLE
    - 트랜잭션을 순차적으로 진행시킨다. 여러 트랜잭션이 동일한 레코드에 동시 접근할 수 없다.
    - 트랜잭션이 순차적으로 처리되어야하므로 처리 성능이 떨어진다.
    - 순수한 select 작업에도 키 락을 읽기 잠금으로 건다. 가장 안전하지만 가장 성능이 떨어진다.
- REPEATABLE READ
    - 변경 전의 레코드를 언두 공간에 백업해둔다. 그러면 변경 전/후 데이터가 모두 존재하므로 여러 버전에 데이터가 존재한다(이를 MVCC, 다중 버전 동시성 제어라고 한다)
    - 백업해둔 데이터 덕분에 롤백된 경우 데이터를 복원할 수 있고 서로 다른 트랜잭션 간 접근할 수 있는 데이터를 제어할 수 있다.
    - 각 트랜잭션은 순차 증가하는 고유한 트랜잭션 번호가 있으며, 어느 트랜잭션에 의해 백업되었는지 트랜잭션 번호를 함께 저장한다.
    - 트랜잭션이 시작되기 전에 커밋된 내용에 대해서만 조회할 수 있는 격리 수준이다. MySQL 에서 기본으로 사용하고 있다.
    - 자신의 트랜잭션 번호보다 낮은 트랜잭션 번호에서 변경된 커밋만 보게된다.
    - 동일한 행을 여러번 읽어도 항상 같은 결과를 보장하는 트랜잭션 격리 수준
    - select for update 를 했을때 문제가 생길 수 있다.
        - 데이터 변경을 위한 조회를 했을때 선택한 행에 잠금을 걸어 다른 트랜잭션이 접근을 하지 못하게 된다.
        - 다른 사용자가 트랜잭션을 시작해 새로운 데이터를 삽입하려하면 삽입은 막지 않는다.
        - 일반적인 경우처럼 undo 로그를 읽지 못하기 때문에 다른 사용자가 삽입한 데이터를 추가로 읽게 된다.
        - 이런 경우를 팬텀 리드라고 한다
- READ COMMITTED
    - 커밋된 완료된 데이터만 조회할 수 있다. **언두 로그** 에서 변경 전의 데이터를 찾아서 반환하게 된다.
    - 반복 읽기를 수행하면 다른 트랜잭션의 커밋 여부에 따라 조회 결과가 달라질 수 있다. 이러한 데이터 부정합 문제를 "반복 읽기 불가능"이라고한다.
    - 일반적인 상황에서는 크게 문제되지 않지만 금전 처리의 경우 문제가 될 수 있다. 총합을 보여주는 트랜잭션이 있는데 계속 입금 작업이 들어온다면 셀렉트 쿼리가 실행될 때마다 다른 결과값을 가져올 것이다.
    - 비반복 읽기 문제가 발생한다. 같은 트랜잭션 내에서 같은 행을 반복해서 읽었을 때 다른 트랜잭션이 그 행을 수정하거나 삭제해서 결과가 달라지는 현상
- READ UNCOMMITED
    - 커밋하지 않은 데이터조차도 접근할 수 있는 격리 수준, 아직 커밋이나 롤백이 되지 않아도 변경된 데이터에 접근해 읽어올 수있다.
    - 트랜잭션의 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있는 부정합 문제를 더티 리드라고 한다.
    - 더티 리드 발생으로 인해 MYSQL에서는 표준에서 인정하지 않을정도로 정합성에 문제가 많다.

## 용어 정리
- 더티 리드 : 아직 커밋되지 않은 데이터가 읽혀서 정합성에 문제가 발생하는 현상 (예 : 나이를 28로 변경, 아직 커밋하지 않음 -> 이걸 읽어감 -> 롤백됨)
- 더티 라이트 : 커밋되지 않은 데이터도 덮어 쓸 수 있음.
- 비반복 읽기 : 하나의 트랜잭션내에서 똑같은 SELECT를 수행했을 경우 항상 같은 결과를 반환해야 하는데 반복하여 읽었을때 다른 결과가 나온다.
- 팬텀 리드 : 한 트랜잭션 내에서 같은 쿼리를 두 번 실행했는데, 다른 트랜잭션에 의해 새로운 데이터 행이 삽입되거나 기존 데이터 행이 삭제되는 현상.
- 스냅숏 격리 : 각 트랜잭션의 아이디를 기반으로 데이터의 어느 버전까지를 읽을 수 있을지를 결정하여 일관된 데이터의 스냅숏을 제공한다.
    - 데이터 읽기 규칙
        1. 트랜잭션 아이디가 더큰 트랜잭션이 쓰거나 커밋한 데이터는 현재 트랜잭션에서 볼 수 없다.
        2. 어보트 된 트랜잭션이 쓰거나 커밋한 데이터는 모두 현재 트랜잭션에서 무시된다.
        3. DB는 각 트랜잭션을 시작할 때 그 시점에서 진행중인 모든 트랜잭션의 목록을 만든다. 이 트랜잭션들이 쓰거나 커밋한 데이터는 모두 현재 트랜잭션에서 무시된다.

# multi column index에서 순서에 따른 차이
인덱스 순서에 따라 쿼리 성능에 차이가 생길 수 있다.
다중 컬럼 인덱스에서 선행하는 컬럼에 대한 조건을 where 에 포함하지 않으면 인덱스를 사용할 수 없다.
어떤 방식의 조회가 자주 일어나는지 필터링이 어떻게 될지 고민이 필요하다.
인덱스 내 컬럼 정렬 방식에 따라서 조회하는데 만약 전공이 수학인 사람을 찾는다면 전공이 수학인 사람 80만명의 비교 연산이 필요하다.
하지만 인덱스 컬럼 순서를 이름, 전공으로 바꾼인덱스에서 이름이 우선적으로 정렬되어있기때문에 이름이 달라지는 학생이 나타나기 전까지만 데이터 스캔이 필요하다.

- 인덱스: (A, B, C)
    - 쿼리: SELECT * FROM table WHERE A = ? AND B = ? → 인덱스 완전히 사용됨.
    - 쿼리: SELECT * FROM table WHERE B = ? AND C = ? → 인덱스 사용 불가.

인덱스 순서에 따라 정렬 성능에도 차이가 생깁니다. (A, B) 인덱스에서 ORDER BY A, B로 정렬하는 경우, 인덱스가 이미 그 순서대로 데이터를 정렬해 두었기 때문에 별도의 추가 작업이 필요 없습니다.
하지만 ORDER BY B, A와 같이 순서가 다르다면 인덱스가 그 순서대로 정렬된 데이터가 아니기 때문에 성능 저하가 발생할 수 있습니다.

선택도가 높은 컬럼을 먼저 인덱스에 넣는 것이 일반적으로 좋습니다. 선택도란 특정 컬럼이 가져오는 값의 다양성입니다. 예를 들어, A 컬럼이 다양한 값을 가진다면, 그 컬럼을 먼저 인덱싱하면 검색 범위를 효과적으로 좁힐 수 있습니다.
자주 사용되는 쿼리를 기준으로 인덱스 순서를 결정하는 것이 좋습니다. 쿼리 패턴을 분석한 후, 조건으로 가장 자주 사용되는 컬럼을 앞에 두는 것이 바람직합니다.

# Spring HttpMessageConverter
**Spring의 HttpMessageConverter**는 HTTP 요청과 응답을 처리할 때, 데이터를 변환하는 역할을 합니다. 구체적으로, Java 객체를 HTTP 요청 본문에 직렬화하거나, HTTP 요청 본문을 Java 객체로 역직렬화하는 일을 담당합니다.

## HttpMessageConverter의 종류
- MappingJackson2HttpMessageConverter: JSON 형식의 데이터를 처리합니다. Java 객체를 JSON으로 변환하거나, JSON을 Java 객체로 변환할 때 사용됩니다. 내부적으로 Jackson 라이브러리를 사용합니다.
- MappingJackson2XmlHttpMessageConverter: XML 형식의 데이터를 처리합니다. Java 객체를 XML로 변환하거나, XML을 Java 객체로 변환합니다.
- StringHttpMessageConverter: String 데이터를 처리합니다. 주로 단순한 텍스트 데이터를 변환할 때 사용됩니다.
- ByteArrayHttpMessageConverter: 바이트 배열 데이터를 처리합니다. 파일 다운로드 등의 경우에 바이트 배열을 사용하여 데이터를 주고받을 때 사용됩니다.
- FormHttpMessageConverter: 폼 데이터를 처리합니다. 주로 application/x-www-form-urlencoded 형식의 폼 데이터를 처리할 때 사용됩니다.

## 어떻게 Instant, LocalDateTime 을 날짜 형식으로 바꿔주는지?
Spring Boot는 기본적으로 Jackson을 사용하여 JSON 형식의 데이터를 처리합니다. Jackson은 LocalDate, LocalDateTime, Instant 등의 Java 8 날짜 및 시간 API를 자동으로 직렬화 및 역직렬화할 수 있습니다. 기본 설정에서는 LocalDateTime을 ISO 8601 표준(예: 2024-09-24T12:34:56)으로 변환하지만, 포맷을 커스터마이징할 수도 있습니다.
```yaml
yaml 에서 직접 설정
spring:
  jackson:
    date-format: yyyy-MM-dd
    time-zone: UTC

response DTO 에서 설정
public class MyDto {

  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd")
  private LocalDateTime date;

  // Getters and Setters
}

ObjectMapper 에 직접 설정
@Bean
  public ObjectMapper objectMapper() {
  ObjectMapper mapper = new ObjectMapper();
  mapper.registerModule(new JavaTimeModule());
  mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
  mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd"));
  return mapper;
}

```
이렇게 해서 커스텀할 수도 있다.

# flyway 스키마 checksum이 어떻게 사용되는지?
Flyway에서 **Checksum**은 데이터베이스 마이그레이션 파일의 무결성을 검증하고 관리하는 중요한 역할을 합니다. Flyway는 마이그레이션 파일이 데이터베이스에 적용될 때마다 그 파일의 Checksum을 계산하고, 이 값을 데이터베이스의 `flyway_schema_history` 테이블에 저장합니다.

### Checksum의 역할 및 사용 방식

1. **Checksum 계산**:
- Flyway는 마이그레이션 파일(예: SQL 스크립트, Java 마이그레이션 등)의 내용을 기반으로 Checksum을 계산합니다. 이 계산된 Checksum은 마이그레이션 파일의 모든 내용(공백, 주석 포함)을 기반으로 한 해시 값입니다.
- Flyway는 기본적으로 **CRC32** 알고리즘을 사용하여 Checksum 값을 계산합니다.

2. **Checksum 저장**:
- 마이그레이션 파일이 처음 실행되면, Flyway는 이 파일의 Checksum을 `flyway_schema_history` 테이블에 기록합니다.
- 이 테이블에는 각 마이그레이션 파일에 대한 **버전**, **파일 이름**, **상태**, **Checksum** 등의 정보가 저장됩니다.

3. **마이그레이션 파일 변경 감지**:
- Flyway는 매번 마이그레이션을 실행할 때, 각 파일의 Checksum을 다시 계산하고 이를 `flyway_schema_history` 테이블에 저장된 기존 Checksum과 비교합니다.
- 만약 파일이 수정되어 Checksum 값이 변경되면, Flyway는 이를 감지하고 마이그레이션 파일이 변경되었음을 인식합니다.
- 파일이 변경된 경우 Flyway는 오류를 발생시키고, 해당 마이그레이션 파일을 다시 실행하지 않도록 합니다. 이를 통해 예기치 않은 데이터베이스 변경을 방지할 수 있습니다.

4. **Checksum 충돌 해결**:
- 만약 마이그레이션 파일이 수정되어 Checksum 충돌이 발생할 경우, Flyway는 이를 해결하기 위해 몇 가지 옵션을 제공합니다:
    - **repair 명령**: Checksum 충돌이 발생한 경우 `flyway repair` 명령을 사용해 `flyway_schema_history` 테이블에 저장된 Checksum을 현재 마이그레이션 파일의 Checksum으로 업데이트할 수 있습니다.
    - **수동으로 파일 복구**: 원본 마이그레이션 파일을 복구하거나, Checksum 충돌 원인을 해결한 후 Flyway를 다시 실행할 수 있습니다.

### Checksum의 주요 특징

- **파일 내용 기반**: Checksum은 파일의 모든 내용을 기준으로 계산됩니다. 즉, 파일 내의 주석, 공백 등도 포함됩니다.
- **파일 보호**: Checksum을 통해 마이그레이션 파일이 무단으로 수정되었거나, 의도치 않게 변경된 경우 Flyway가 이를 감지할 수 있습니다.
- **변경 방지**: 이미 적용된 마이그레이션 파일을 수정하는 것은 위험할 수 있기 때문에, Checksum을 통해 변경을 방지하는 역할을 합니다.

### Checksum 충돌이 발생할 수 있는 상황
- **파일 수정**: 이미 적용된 마이그레이션 파일을 수정하는 경우. 예를 들어, 개발 중 SQL 파일의 내용이 변경된 경우.
- **파일 이동**: 마이그레이션 파일을 다른 위치로 이동시키거나 파일 이름을 변경하는 경우에도 Checksum 충돌이 발생할 수 있습니다.

### Checksum이 중요한 이유
- **데이터 무결성 보장**: 마이그레이션 파일이 예기치 않게 변경되지 않도록 하여 데이터베이스의 무결성을 유지합니다.
- **재현성 확보**: 동일한 Checksum을 기반으로 마이그레이션이 일관되게 적용되도록 합니다.

### 요약
- Flyway의 Checksum은 마이그레이션 파일의 내용 변경을 감지하기 위해 사용됩니다.
- 마이그레이션 파일이 처음 적용될 때 Flyway는 그 파일의 Checksum을 `flyway_schema_history` 테이블에 저장하고, 이후에 변경되었는지 확인하는 데 사용됩니다.
- Checksum 충돌이 발생하면 Flyway는 경고를 발생시키고, 이를 해결하기 위해 `repair` 명령을 사용할 수 있습니다.
