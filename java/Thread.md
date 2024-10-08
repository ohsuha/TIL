# Thread
> 프로세스 내에서 실행되는 개별적인 작업 단위. 프로세스와 같은 메모리 공간을 공유하므로, 같은 프로세스 내에서 데이터와 자원을 공유할 수 있다.
 
- JVM은 적어도 32~64 MB의 메모리를 점유하지만 스레드는 1MB이내의 메모리를 점유한다. 경량 프로세스 라고도 한다.
- 스레드가 어떤 작업을 할지 구현하는 메소드는 run() 이다. run() 메소드가 종료되면 새로 생성된 스레드도 종료된다.
- 스레드를 시작하게 하는 메소드는 start() 이다.
- start()가 실행되면 새로운 호출 스택을 생성한 다음 run() 을 새로운 호출 스택에서 실행한다.
- 스레드는 독립적인 작업을 수행하기 위해 자신만의 호출 스택을 필요로 하기 때문이다.
- 스케쥴러가 스레드를 돌며 우선순위를 고려하여 실행 순서와 시간을 결정하고 주어진 시간동안 작업을 마치지 못한 스레드는 다시 자신의 차례가 올때까지 대기상태로 있는다.
- 한 스레드에서 예외가 발생해 종료되어도 다른 스레드의 실행에는 영향을 미치지 않는다.
- 스레드에 매개변수를 전달하고자 할 때는 생성자 매개 변수를 통해 전달할 수 있다.
- 스레드 종료는 반드시 interrupt() 를 사용, stop() 을 사용하지 말자 stop 은 deprecated 되었다.
- 두 스레드가 서로 다른 자원을 사용하는 작업일때 멀티스레드가 더 효율적이다.
- 스레드에 우선순위를 정할 수 있지만 멀티코어에서는 OS의 스케쥴링을 따르기 때문에 우선순위에 따른 차이가 거의없다.
  - 스레드에 우선순위 대신 작업에 우선순위를 두어 priority queue 같은 곳에 저장해놓고 우선순위가 높은 작업이 먼저 처리되도록 하는 것이 났다.

## 스레드 그룹
- 자신이 속한 스레드 그룹이나 하위 스레드 그룹만 변경할 수 있고, 다른 스레드 그룹의 스레드를 변경할 수 없게 하기 위해서 만들어졌다.
- 모든 스레드는 반드시 스레드 그룹에 포함되어야한다.
- JVM 은 최초에 main / system 이라는 스레드 그룹을 만들고 JVM 운영에 필요한 스레드들을 생성해 이 그룹에 넣는다.
- main 은 main 그룹에, 가비지 컬렉션을 수행하는 finalized 스레드는 system 스레드 그룹에 속한다.
- 우리가 생성하는 모든 스레드 그룹은 main 스레드 그룹의 하위 스레드 그룹이며 스레드 그룹을 지정하지 않고 생성한 스레드는 자동으로 main 그룹에 들어간다.

## 스레드의 진행 상태
![img](https://joont92.github.io/temp/thread-states.png)

| 메소드            | 설명                                                                 | 상태 변경 방식         |
|-------------------|----------------------------------------------------------------------|------------------------|
| `start()`         | 새로운 스레드를 시작하고, 스레드를 **RUNNABLE** 상태로 전환합니다.    | **NEW** -> **RUNNABLE**|
| `run()`           | 스레드가 실행할 작업을 정의합니다. 직접 호출 시 스레드는 새로운 스레드가 아니라 현재 스레드에서 실행됩니다. | 상태 변화를 일으키지 않음 |
| `sleep(long ms)`  | 스레드를 **TIMED_WAITING** 상태로 전환하고 지정된 시간 동안 일시 정지시킵니다.  | **RUNNABLE** -> **TIMED_WAITING** |
| `interrupt()`     | `sleep()`, `wait()`, `join()` 등으로 대기 중인 스레드를 깨웁니다.    | **WAITING** or **TIMED_WAITING** -> **RUNNABLE** |
| `join()`          | 현재 스레드가 다른 스레드의 종료를 기다리도록 하며 **WAITING** 상태로 전환합니다.  | **RUNNABLE** -> **WAITING** |
| `yield()`         | 현재 실행 중인 스레드를 일시적으로 양보하고 다른 스레드에게 CPU를 넘깁니다. | **RUNNABLE** -> **RUNNABLE** |
| `wait()`          | 스레드를 **WAITING** 상태로 전환하며, 다른 스레드에서 `notify()` 또는 `notifyAll()`이 호출될 때까지 대기합니다. | **RUNNABLE** -> **WAITING** |
| `notify()`        | **WAITING** 상태의 스레드를 깨워 **RUNNABLE** 상태로 만듭니다.         | **WAITING** -> **RUNNABLE** |
| `notifyAll()`     | **WAITING** 상태의 모든 스레드를 깨워 **RUNNABLE** 상태로 만듭니다.    | **WAITING** -> **RUNNABLE** |
| `synchronized`    | 지정된 코드 블록이나 메서드를 한 번에 하나의 스레드만 실행할 수 있도록 합니다. | 상태 변화를 일으키지 않음 |
| `isAlive()`       | 스레드가 **RUNNABLE** 또는 **BLOCKED** 상태인지 여부를 확인합니다.    | 상태 변화를 일으키지 않음 |
| `setDaemon()`     | 데몬 스레드로 설정합니다. 데몬 스레드는 자바 프로그램이 종료되면 자동으로 종료됩니다. | 상태 변화를 일으키지 않음 |

### interrupt()의 작동 방식:
스레드가 sleep()이나 wait() 같은 메소드로 대기 중일 때, interrupt()가 호출되면 스레드는 InterruptedException을 던지며 대기에서 깨어나게 됩니다. 하지만 스레드가 RUNNING 상태로 작업을 하고 있을 경우, interrupt()는 단순히 인터럽트 플래그를 설정하는 역할을 하며 스레드를 강제로 종료시키지는 않습니다. 스레드가 이 플래그를 직접 확인하여 적절한 종료 처리를 해야 합니다.
1. 스레드가 대기 중일 때: sleep(), wait(), join() 등의 메소드를 호출 중일 때 interrupt()가 호출되면 **InterruptedException**이 발생하고, 스레드는 예외 처리를 통해 제어권을 얻습니다.
2. 스레드가 실행 중일 때: 스레드가 실행 중이라면 interrupt()는 단순히 인터럽트 플래그를 설정합니다. 스레드가 이 플래그를 확인하고 작업을 중단할지 여부를 결정해야 합니다. 

따라서, 스레드가 스스로 종료되도록 하려면 스레드 내부에서 주기적으로 Thread.interrupted() 또는 isInterrupted() 메서드를 확인하고, 플래그가 설정된 경우 스레드 종료 로직을 구현하는 방식으로 처리해야 합니다.

### suspend() resume() stop() 이 deprecated 된 이유
1. **suspend()**
   - Thread.suspend() 메소드는 스레드를 일시 중단(suspend)시킵니다. 하지만 이 메소드는 스레드의 실행을 강제로 중단시키기 때문에, **스레드가 자원을 점유한 상태에서 중단되면 교착 상태(Deadlock)** 를 일으킬 수 있습니다. 
   - 스레드가 자원을 점유하고 있을 때 중단되면, 해당 자원은 다른 스레드에 의해 해제되지 않고, 결국 시스템이 교착 상태에 빠질 수 있습니다. 이는 프로그램의 불안정성을 야기합니다. 
   - 예를 들어, 스레드가 잠금(lock)을 획득한 상태에서 중단되면, 그 잠금은 풀리지 않아서 다른 스레드가 더 이상 그 자원에 접근하지 못하게 됩니다.
2. **resume()**
   - Thread.resume() 메소드는 suspend()에 의해 중단된 스레드를 다시 실행 상태로 복구하는 메소드입니다. 하지만 이 메소드 또한 교착 상태와 같은 문제를 해결하지 못합니다.
   - 만약 resume()이 호출되기 전에 스레드가 자원을 점유한 상태에서 중단되었다면, 다른 스레드가 해당 자원에 접근하지 못한 상태에서 자원이 잠겨버리므로 여전히 교착 상태가 발생할 수 있습니다.
   - suspend()와 resume()은 일종의 쌍으로 작동해야 하는데, 중단된 시점이 어디인지, 안전하게 재개할 수 있는지 판단하기가 어렵습니다.
3. **stop()**
   - Thread.stop() 메소드는 즉시 스레드 실행을 중단시키는 메소드입니다. 그러나 이 메소드는 매우 위험합니다. 스레드를 즉시 중단시키면, 스레드가 수행하던 작업을 안전하게 마무리할 기회를 주지 않으며, 데이터 손실이나 일관성 문제를 일으킬 수 있습니다. 
   - 스레드가 중요한 자원(파일, 네트워크 연결 등)을 사용 중이거나, 데이터의 무결성을 유지하기 위한 중요한 코드 부분을 실행 중일 때 stop()을 호출하면, 자원이 해제되지 않고 시스템이 불안정한 상태에 빠질 수 있습니다.
   - 예를 들어, 객체의 상태를 변경하는 도중에 스레드가 중단되면, 객체는 일관성이 없는 상태에 남아 있을 수 있습니다.


## Runnable 인터페이스 구현을 통한 스레드 생성
```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Running in thread: " + i);
            try {
                Thread.sleep(1000); // 1초 대기
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        MyRunnable runnable = new MyRunnable();
        Thread thread = new Thread(runnable);
        thread.start(); // 스레드 시작
    }
}
```

## Thread 클래스 상속을 통한 스레드 생성
```java
public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Running in thread: " + i);
            try {
                Thread.sleep(1000); // 1초 대기
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start(); // 스레드 시작
    }
}
```

### 왜 두가지 방식을 제공할까?
- extends 는 다중상속이 불가능하므로 다른 클래스를 상속 중일 경우 Thread 를 상속할 수 없기 떄문에, 상황에 따라 적절히 사용하자.

## 데몬스레드
데몬 스레드는 백그라운드에서 실행되며, 프로그램의 주요 작업과는 별도로 실행되는 스레드. 프로그램이 종료되면 자동으로 종료된다.
다른 스레드가 모두 종료되어야 프로그램이 종료 될 수 있는데 데몬스레드는 실행되어도 프로그램이 종료될 수 있다.
자동저장이나 화면 자동 갱신, 가비지 컬렉터에 쓰인다.
```java
public class DaemonThreadExample {
    public static void main(String[] args) {
        Thread daemonThread = new Thread(() -> {
            while (true) {
                try {
                    Thread.sleep(1000);
                    System.out.println("Daemon thread running...");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // 데몬 스레드로 설정
        daemonThread.setDaemon(true);
        daemonThread.start();

        // 메인 스레드의 작업
        try {
            Thread.sleep(5000); // 메인 스레드 5초 대기
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread ending...");
    }
}
```
# 스레드의 동기화
## synchronized
> 멀티스레드 환경에서 공유 자원의 동기화를 보장하기 위해 사용되는 키워드

- 메소드에서 인스턴스 변수를 수정하려고 할 때 여러 스레드가 한 객체에 선언된 메소드에 접근하여 데이터를 처리하려고 할 때 동시에 연산을 수행하여 값이 꼬일 수 있다.
- synchronized는 동기화를 제공하지만, 너무 많은 동기화가 성능을 저하시킬 수 있다. 적절한 범위에서 사용해야한다. 필요한 부분만 동기화하고 나머지는 비동기적으로 처리하는 게 좋다.
- 한 스레드가 진행 중인 작업을 다른 스레드가 간섭하지 못하도록 막는 것을 스레드의 동기화 라고 한다.


### 메소드에 적용
메서드 전체를 동기화하여 해당 메서드를 동시에 하나의 스레드만 접근
```java
public class SynchronizedExample {
    private int counter = 0;

    // 동기화 메서드
    public synchronized void increment() {
        counter++;
    }

    public int getCounter() {
        return counter;
    }

    public static void main(String[] args) {
        SynchronizedExample example = new SynchronizedExample();
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Counter: " + example.getCounter());
    }
}
```

### synchronized 블록 사용
동시성 이슈가 발생할 수 있는 부분만 Synchronized 처리를 해준다.
```java
public class SynchronizedBlockExample {
    private int counter = 0;

    public void increment() {
        synchronized (this) {
            counter++;
        }
    }

    public int getCounter() {
        return counter;
    }

    public static void main(String[] args) {
        SynchronizedBlockExample example = new SynchronizedBlockExample();
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Counter: " + example.getCounter());
    }
}
```
## wait() 과 notify()
- 특정 스레드가 객체의 락을 가진 상태로 오랜 시간을 보내지 않도록 하는 것도 중요하다.
- 동기화된 임계 영역의 코드를 수행하다가 작업을 더이상 진행할 상황이아니면 일단 wait() 을 호출하여 스레드가 락을 반납하고 기다리게 한다.
- 그러면 다른 스레드가 락을 얻어 해당 객체에 대한 작업을 수행할 수 있게 된다. 나중에 작업을 진행할 수 있는 상황이 되면 notify() 를 호출해서, 작업을 중단했던 스레드가 다시 락을 얻어 작업을 진행할 수 있게 한다.
- 다음 사람에게 순서를 양보하고 기다리다가 자신이 원하는 빵이 나오면 통보를 받고 빵을 사가는 것이다.
- 오래기다린 스레드가 락을 얻는 다는 보장은 없다. wait()이 호출되면 실행 중이던 스레드는 해당 객체의 대기실(waiting pool)에서 통지를 기다린다.
- notify() 가 실행되면 대기실에 있던 모든 스레드 중에서 임의의 스레드만 통지를 받는다.
  - notifyAll() 은 기다리고 있던 모든 스레드에게 통보를 하긴한다 하지만 Lock 을 얻을 수 있는 것은 하나의 스레드일 뿐이고 나머지는 다시 lock 을 기다린다.
- waiting pool 은 객체마다 존재하는 것이므로 notifAll()이 호출된다고 해서 모든 객체의 waiting pool 에 있는 스레드가 깨워지는 것은 아니다.
- notifyAll() 이 호출된 객체의 waiting pool 에 대기중인 스레드만 해당된다는 것을 기억하자.
- 지나치게 운이 나쁘면 어떤 스레드는 계속 통지 받지 못하고 오랫동안 기다리게 되는데 이것을 기아상태라고한다.
- notifyAll 로 이것을 풀어주면 스레드들이 lock 을 얻기 위해 경쟁하는데 이것을 경쟁상태라고 한다.
- 경쟁상태를 개선하기 위해서는 스레드들을 구별해서 통지하는 것이 필요한데 lock 과 condition 을 통해 선별적으로 통지할 수 있다.

## Lock
- JDK 1.5 부터 추가된 java.util.concurrent.locks 패키지가 제공하는 lock 클래스들을 이용하는 방법이 있다.
### 1. **ReentrantLock**
**설명**: `ReentrantLock`은 재진입이 가능한 배타적 락(exclusive lock)으로, 동일한 스레드가 여러 번 잠금을 획득할 수 있습니다. 스레드가 특정 조건에서 락을 해제하고 다시 획득하여 임계 영역에 접근할 수 있는 유연성을 제공합니다.
무조건 락이 있어야만 임계 영역의 코드를 수행할 수 있다.

#### **장점**:
- **재진입 가능**: 동일한 스레드가 여러 번 락을 획득할 수 있어, 재진입이 필요한 복잡한 로직을 처리할 때 유리합니다.
- **공정성 옵션**: 락을 획득하는 순서를 지정할 수 있는 공정성(fairness) 옵션을 제공합니다. 공정한 락은 먼저 기다린 스레드가 우선적으로 락을 획득합니다.
- **Condition 지원**: 스레드가 특정 조건에서 기다리게 할 수 있는 `Condition` 객체를 제공합니다. 이를 통해 세밀한 동기화 제어가 가능합니다.

#### **단점**:
- **락 경합**: 여러 스레드가 동시에 락을 획득하려고 시도하면 경합이 발생하고, 이로 인해 성능 저하가 생길 수 있습니다.
- **쓰기 중심**: `ReentrantLock`은 주로 쓰기 중심의 락 메커니즘을 제공하며, 읽기와 쓰기 작업을 구분할 수 없습니다.

#### **적합한 경우**:
- 임계 영역을 제어할 때, **하나의 스레드만** 자원에 접근해야 하는 상황에서 적합합니다.
- **재진입이 필요한 복잡한 코드**(예: 중첩된 락 구조)에서 유용합니다.

### 2. **ReentrantReadWriteLock**
**설명**: `ReentrantReadWriteLock`은 읽기 작업에는 공유적이고, 쓰기 작업에는 배타적인 특성을 가진 락입니다. 여러 스레드가 동시에 읽기 작업을 수행할 수 있지만, 쓰기 작업 중에는 다른 읽기 또는 쓰기 작업이 차단됩니다.
읽기 락이 걸려있으면, 다른 스레드가 읽기 락을 중복해서 걸고 읽기를 수행할 수 있다. 읽기는 내용을 변경하지 않으므로 동시에 여러 쓰레드가 읽어도 문제되지 않는다. 그러나 읽기 락이 걸린 상태에서 쓰기 락을 거는 것은 허용되지 않는다.

#### **장점**:
- **읽기-쓰기 분리**: 읽기와 쓰기 작업을 명확히 구분하여, 읽기 작업은 여러 스레드에서 동시 수행할 수 있습니다.
- **성능 향상**: 다수의 읽기 작업이 동시에 발생할 때 성능을 최적화합니다. 쓰기 작업이 적고 읽기 작업이 많은 상황에서 매우 유리합니다.
- **재진입 가능**: `ReentrantLock`과 마찬가지로 동일한 스레드가 여러 번 락을 획득할 수 있습니다.

#### **단점**:
- **경합 발생 가능성**: 쓰기 작업이 잦을 경우, 읽기 작업과의 경합으로 인해 성능 저하가 발생할 수 있습니다. 읽기와 쓰기 작업의 비율이 잘못 설정되면 효율이 떨어집니다.
- **잠금 전환 불가능**: 쓰기 락에서 읽기 락으로, 또는 그 반대의 전환이 불가능합니다. 따라서 쓰기와 읽기를 빈번하게 전환해야 하는 경우는 비효율적입니다.

#### **적합한 경우**:
- **읽기 작업이 매우 빈번하고**, 쓰기 작업은 적은 경우에 적합합니다. 예를 들어, 캐시 시스템 또는 다수의 클라이언트가 동시에 데이터를 조회하는 상황에서 효과적입니다.
- 여러 스레드가 읽기 작업을 동시에 수행할 수 있지만, **데이터 일관성**이 중요한 환경에서 사용됩니다.

### 3. **StampedLock**
**설명**: `StampedLock`은 `ReentrantReadWriteLock`에 **낙관적 읽기 잠금(Optimistic Read Lock)** 기능을 추가한 잠금 메커니즘입니다. 읽기와 쓰기 락 외에도, 데이터가 실제로 변경되지 않을 것이라고 가정하고 읽기를 시도하는 낙관적 읽기 잠금을 제공합니다.
- 락을 걸거나 해지할 때 스탬프(long 타입의 정수값) 을 사용하며 읽기와 쓰기를 위한 락 외에 `낙관적 읽기 락` 이 추가된 것이다. 
- 읽기 락이 걸려있으면, 쓰기 락을 얻기 위해서는 읽기 락이 풀릴때까지 기다려야하는데 비해 낙관적 읽기 락은 쓰기 락에 의해 바로 풀린다. 
- 그래서 낙관적 읽기에 실패하면, 읽기 락을 얻어서 다시 읽어와야 한다. 무조건 읽기 락을 걸지 않고, 쓰기와 읽기가 충돌할 때만 쓰기가 끝난 후에 읽기 락을 거는 것이다. 
- StampedLock 의 가장 큰 장점 중 하나는 낙관적 읽기 잠금을 제공한다는 점입니다. 이는 데이터에 실제로 변경이 일어나지 않는다고 가정하고, 읽기 작업을 수행하는 방식입니다. 
- 읽기 도중에 데이터가 변경되지 않으면 별도의 재확인이 필요 없으므로 매우 빠르게 읽기 작업을 처리할 수 있습니다. 
- 만약 읽기 도중 데이터가 변경되었으면, 읽기 작업을 재시도하거나 다른 형태의 잠금을 획득해야 합니다.

#### **장점**:
- **낙관적 읽기 잠금**: 읽기 작업이 쓰기 작업과 충돌하지 않을 때 성능을 크게 향상시킬 수 있습니다. 낙관적 읽기 잠금은 쓰기 작업이 발생하지 않는다고 가정하여, 락을 걸지 않고 읽기를 시도합니다. 만약 읽기 도중 데이터가 변경되지 않으면 성능 저하 없이 읽기를 완료할 수 있습니다.
- **성능 최적화**: 특히 다수의 읽기 작업이 발생하고, 쓰기 작업이 상대적으로 적은 경우에 큰 성능 향상을 기대할 수 있습니다. 락이 불필요할 때 낙관적으로 읽기를 처리하고, 필요 시에만 실제 읽기 또는 쓰기 잠금을 획득하는 구조입니다.
- **잠금 상태 전환 가능**: 읽기 잠금을 쓰기 잠금으로 전환하거나, 쓰기 잠금을 읽기 잠금으로 전환하는 것이 가능합니다. 이 기능은 잠금을 적절히 전환함으로써 성능을 더욱 최적화할 수 있습니다.

#### **단점**:
- **비재진입성**: `StampedLock`은 **비재진입 잠금**입니다. 즉, 동일한 스레드가 이미 획득한 잠금을 다시 요청할 수 없습니다. 이로 인해 복잡한 재진입 구조의 코드에서는 주의가 필요합니다.
- **복잡성 증가**: `StampedLock`은 낙관적 읽기와 쓰기 잠금 전환 등의 기능을 제공하지만, 사용자가 직접 잠금과 해제를 관리해야 하므로 코드가 복잡해질 수 있습니다. 스탬프를 사용하여 잠금 상태를 확인하는 과정이 추가됩니다.
- **낙관적 읽기 실패 가능성**: 낙관적 읽기 잠금이 실패하면 다시 읽기 잠금을 획득해야 하는데, 이 경우 성능 손실이 발생할 수 있습니다.

#### **적합한 경우**:
- **읽기 작업이 많고 쓰기 작업이 상대적으로 적은 시스템**에서 매우 유리합니다. 데이터가 거의 변경되지 않으며, 주로 읽기 작업이 빈번한 환경에서 최적의 성능을 발휘할 수 있습니다.
- **낙관적 락이 유리한 경우**: 데이터가 변경되지 않을 확률이 높은 시나리오에서는 낙관적 읽기를 통해 성능을 크게 향상시킬 수 있습니다. 예를 들어, 데이터가 적은 빈도로 변경되는 캐시 시스템이나, 데이터베이스 조회 작업에 적합합니다.

### **결론: 어떤 경우에 사용할지**
- **ReentrantLock**: 단순한 락 구조가 필요하고, 쓰기 작업 위주의 동기화가 필요한 경우. **재진입이 필요한 복잡한 코드**에서 유리하며, 공정성 제어가 필요할 때 적합합니다.
- **ReentrantReadWriteLock**: **읽기 작업이 많고, 쓰기 작업이 적은 상황**에서 사용됩니다. 여러 스레드가 동시에 데이터를 읽을 수 있지만, 데이터 일관성을 보장하고 싶을 때 유용합니다.
- **StampedLock**: **읽기 작업이 대부분이고, 쓰기 작업이 매우 적은 경우**에 최적입니다. 특히, 데이터를 읽는 동안 변경이 거의 발생하지 않으면 **낙관적 읽기 잠금**을 사용해 성능을 극대화할 수 있습니다.

### 생성자와 주요 메소드
| 생성자/메서드       | 설명                                                                                                     |
|---------------------|--------------------------------------------------------------------------------------------------------|
| **생성자**          |                                                                                                        |
| `ReentrantLock()`   | 기본적으로 비공정한(non-fair) 락을 생성합니다. 대기 중인 스레드 중 하나가 랜덤하게 락을 획득할 수 있습니다.                                     |
| `ReentrantLock(boolean fair)` | 공정성 모드를 지정하여 락을 생성합니다. `true`로 설정하면 공정성 모드가 활성화되어, 락을 기다리는 순서대로 스레드가 락을 획득합니다. 공정성 체크로 인해 성능이 떨어진다.    |
| **주요 메서드**     |                                                                                                        |
| `lock()`            | 락을 획득할 때까지 현재 스레드를 대기 상태로 만듭니다. 락을 획득하면 이후의 임계 영역 코드를 실행할 수 있습니다.                                      |
| `unlock()`          | 현재 스레드가 가지고 있는 락을 해제합니다. 락을 획득한 스레드만 이 메서드를 호출할 수 있습니다.                                                |
| `tryLock()`         | 락을 즉시 시도해서 획득하면 `true`를 반환하고, 획득하지 못하면 `false`를 반환합니다. 대기하지 않습니다.                                      |
| `tryLock(long time, TimeUnit unit)` | 주어진 시간 동안 락을 시도합니다. 시간 안에 획득하면 `true`, 실패하면 `false`를 반환합니다.                                            |
| `lockInterruptibly()` | 락을 기다리는 동안 인터럽트(Interrupt)를 처리할 수 있습니다. 인터럽트가 발생하면 `InterruptedException`이 발생하여 대기 중인 스레드가 취소될 수 있습니다. |
| `isLocked()`        | 현재 락이 잠겨있는지 여부를 반환합니다. `true`이면 다른 스레드가 락을 획득 중입니다.                                                    |
| `isFair()`          | 이 락이 공정성 모드로 생성되었는지 여부를 반환합니다. 공정성 모드에서는 락을 대기 중인 스레드가 먼저 락을 획득합니다.                                    |
| `getHoldCount()`    | 현재 스레드가 획득한 락의 횟수를 반환합니다. 재진입이 가능한 락이므로, 중첩된 락 획득 횟수를 확인할 수 있습니다.                                      |
| `isHeldByCurrentThread()` | 현재 스레드가 이 락을 보유하고 있는지 여부를 반환합니다. `true`이면 현재 스레드가 락을 보유 중입니다.                                          |
| `hasQueuedThreads()` | 락을 기다리고 있는 스레드가 있는지 여부를 반환합니다. 대기 중인 스레드가 있으면 `true`를 반환합니다.                                           |
| `hasQueuedThread(Thread thread)` | 주어진 스레드가 락을 대기 중인지 여부를 반환합니다. 특정 스레드가 대기열에 있는지 확인할 때 사용됩니다.                                            |

- unlock() 은 try-finally 문으로 감싸서 사용하는 것이 일반적이다.
- lock() 은 lock 을 얻을 때까지 스레드를 블락 시키므로 스레드의 응답성이 나빠질 수 있다. tryLock()을 통해서 지정된 시간동안 lock을 얻지 못하면 다시 작업을 시도할 것이닞 포기할 것인지를 사용자가 결정할 수 있게 하는것이 좋다.
  - InterruptedException 을 발생시킬 수 있는데, 이것은 지정된 시간동안 lock 을 얻으려고 기다리는 중에 interrupt()에 의해 작업을 취소될 수 있도록 코드를 작성할 수 있게 해준다.

## Condition
공유객체의 waiting pool 에 같이 몰아 넣는 대신, A 쓰레드를 위한 condition 과 B 스레드를 위한 condition 을 만들어서 각각의 waiting pool 에서 따로 기다리록 해서 깨울 수 있다.
```java
private ReentrantLock lock = new ReentrantLock();

private Condition a = lock.newCondition();
private Condition b = lock.newCondition();
```
이렇게 지정하고 난 다음에는 wait() & notify() 대신 Condition 의 await() & signal() 을 사용하면 그걸로 끝이다.

# volatile
- 멀티 코어 프로세서에서는 코어마다 별도의 캐시를 가지고 있다. 
- 코어는 메모리에서 읽어온 값을 캐시에 저장하고 캐시에서 값을 읽어서 작업한다. 다시 같은 값을 읽어올 떄는 먼저 캐시에 있는지 확인하고 없을때만 메모리에서 읽어온다. 
- 그러다보니 도중에 메모리에 저장된 변수의 값이 변경되었는데도 캐시에 저장된 값이 갱신되지 않아서 메모리에 저장된 값이 다른 경우가 발생한다.
- volatile 을 변수 앞에 붙이면 코어가 변수의 값을 읽어올 떄 캐시가 아닌 메모리에서 읽어오기 때문에 캐시와 메모리간의 값의 불일치가 해결된다.
- synchronized 블럭을 사용해도 같은 효과를 얻을 수 있는데, 스레드가 synchronized 블럭으로 들어갈 때와 나올 때, 캐시와 메모리간의 동기화가 이뤄지기 때문이다.
- 상수(final) 에는 volatile 을 붙일 수없다. 상수는 변하지 않는 값이므로 멀티쓰레드에 안전하다. 그래서 붙일 필요도 없다.

### long과 double 의 원자화
JVM은 데이터를 4바이트 단위로 처리하기 때문에 int 보다 작은 타입들은 한 번에 읽거나 쓰는게 가능하다.
하지만 8바이트인 long 과 double 타입은 나누어 처리하게 된다. 이때 다른 작업이 끼어들면 숫자가 변경될 가능성이 생긴다.
다른 스레드가 끼어들지 못하게 하려고 싱크로나이즈드 블럭을 사용할 수도 있지만, volatile 을 붙여 해결할 수있다.
volatile 은 해당 변수에 대한 읽기 쓰기가 원자화(작업을 더이상 나눌 수 없게 한다) 된다. 단 동기화 하는 것은 아니라서 동기화가 필요할때는 싱크로 나이즈드 블록을 반드시 사용해 동기화 해야한다.

# fork & join 프레임웍
- 하나의 작업을 작은 단위로 나눠서 여러 쓰레드가 동시에 처리하는 것을 쉽게 만들어 준다.
- RecursiveAction(반환값이 없는 작업을 구현), RecursiveTask(반환값이 있는 작업을 구현) 두 클래스 중에 하나를 상속받아 구현한다.
- compute() 라는 추상 메서드를 가지고 있는데 상속을 통해 이 추상메서드를 구현하면 된다.
```java
class SumTask extends RecursiveTask<Long> {
	long from, to;
	SumTask(long from, long to) {
		this.from = from;
		this.to = to;
    }
	public Long compute() {
		// 처리할 작업을 수행하기 위한 문장을 넣는다.
    }
}

//호출 부분
ForkJoinPool pool = new ForkJoinPool();
SumTask task = new SumTask(from, to);
Long result = pool.invoke(task);
```
fork&join 프레임웍으로 수행할 작업도 invoke() 로 실행해야한다.

## compute() 구현
```java
public Long compute() {
	long size = to - from + 1;
	if (size <= 5) return sum;
	
	long half = (from+to)/2;
	
	SumTask leftSum = new SumTask(from, half);
	SumTaks rightSum = new SumTask(half+1, to);
	
	leftSum.fork(); // 작업을 작업 큐에 넣는다.
    return rightSum.compute() + leftSum.join();
}
```
- compute() 가 처음으로 호출되면 더할 숫자의 범위를 반으로 나눠서 한쪽에는 fork() 를 호출해 작업 큐에 저장한다.
- 하나의 스레드는 compute() 를 재귀호출하면서 작업을 계속해서 반으로 나누고, 다른 스레드는 fork()에 의해서 작업 큐에 추가된 작업을 수행한다.
- 작업 훔쳐오기 : fork() 가 호출되어 작업큐에 추가된 작업 역시 compute() 에 의해서 더 이상 나눌 수 없을떄까지 반복해서 나뉘고, 자신의 작업 큐가 비어있는 스레드는 다른 스레드의 작업큐에서 작업을 가져와 수행한다.
  - 이 과정은 모두 스레드풀에 의해 자동적으로 이루어진다. 

## fork() 와 join()
fork() 는 작업을 스레드의 작업 큐에 넣는 것이고, 작업 큐에 들어간 작업은 더 이상 나눌 수 없을때까지 나뉜다.
즉 compute() 로 나뉘고 fork()로 작업 큐에 넣는 작업이 계속해서 반복되는데, 나눠진 작업은 각 스레드가 골고루 나눠서 처리하고 작업의 결과는 join()을 호출해서 얻을 수 있다.
- fork() : 해당 작업을 스레드 풀의 작업 큐에 넣는다. 비동기 메서드
- join() : 해당 작업의 수행이 끝날 때까지 기다렸다가. 수행이 끝나면 그 결과를 반환한다. 동기 메서드
- 위의 코드에서 Return 문에서 compute() 가 재귀호출될 때, join() 은 호출되지 않는다. 그러다가 작업을 더 이상 나눌 수 없게 되었을때 compute() 의 재귀 호출은 끝나고 join()의 결과를 기다렸다가 더해서 결과를 반환한다.
- 재귀 호출된 compute() 가 모두 종료될 때, 최종 결과를 얻는다.

# Thread Pool : java, tomcat, spring boot 에서의 스레드 풀, WAS 관점에서
프로세스의 작업을 스레드 단위로 나누어 할 수 있는데, 나눠진 프로세스의 작업들을 CPU 코어가 Unit of Execution 단위로 처리할 수 있다.
요청이 올때마다 스레드를 생성하고 처리하게 되면 두가지 문제가 발생한다.
1. 스레드 자체가 생성비용이 크기 때문에 요청에 대한 응답시간이 늘어난다.
   - 자바는 One-to-One Threading model 로 스레드를 생성한다.
   - One-to-One Threading model 은 Process 스레드 생성시 OS 레벨의 스레드와 연결해야한다.
   - 새로운 스레드를 생성할때마다 OS 커널의 작업이 필요하다.
2. 스레드 자체가 너무 많아졌을때 발생하는 문제
   - 처리 속도보다 빠르게 요청이 쏟아져 들어오면 새로운 스레드가 무제한적으로 계속 생성된다.
   - 스레드가 많아질 수록 컨텍스트 스위칭이 더 자주 발생하므로 CPU 오버헤드가 증가한다.
## 스레드 풀
스레드를 허용된 개수 안에서 사용하도록 제한하는 시스템이다. pool 에 일정량의 스레드를 모아 놓고 일정량의 스레드만 사용하는 것이다.
스레드풀은 크게 작업큐와 스레드로 나눌 수 있다. 한번 사용된 스레드가 없어지지 않고 재사용이 가능하다.
1. 미리 만들어놓은 스레드를 재사용할 수 있기 때문에 새로운 스레드를 생성하는 비용을 줄일 수 있다.
2. 사용할 스레드 개수를 제한하기 때문에 무제한적으로 스레드가 생성되지 않아서 방지 가능하다.

## 자바에서의 스레드풀 구현
ThreadPoolExecutor 를 통해서 구현한다. static 메서드로 다양한 형태의 스레드풀을 제공한다.

`ExecutorService threadPool = Executors.newFixedThreadPool(10);`

- maximumPoolSize : 스레드의 최대 갯수
- corePoolSize : 요청이 적을때는 많은 양의 스레드를 가지고 있을 필요가 없어서 keepAliveTime 이후로 corePoolSize 만큼 줄일 수 있다.
- keepAliveTime : 대기 시간

## Tomcat 의 스레드풀
Java 의 스레드풀 클래스와 유사하다.
- maxConnections : 톰캣이 최대로 동시에 처리할 수 있는 커넥션의 수. 웹 요청이 들어오면 톰캣의 커넥터가 커넥션을 생성하면서 요청된 작업을 스레드 풀의 스레드에 연결한다.
- acceptCount : maxConnections 이상의 요청이 들어왔을때 사용하는 대기열 Queue 의 사이즈, maxConnections 와 acceptCount 가 가득차면 요청이 거절될 수도 있다.

## 스프링부트 설정을 통한 tomcat thread pool 설정
`server.tomcat.threads.max`

스레드풀에서 사용할 최대 스레드 개수, 기본값은 200이다.
서버 어플리케이션이 동시에 처리할 수 있는 요청 개수와 관련이 있다.

`server.tomcat.threads.min-spare`

스레드에서 최소한으로 유지할 스레드 개수, 기본값은 10이다.
잘못 설정하면 사용하지 않는 스레드가 메모리를 차지해 비효율이 발생한다.

`server.tomcat.max-connections`

서버의 실질적인 동시요청 처리 개수, 기본값은 8192
블로킹방식 : 커넥션 하나가 스레드 하나와 연결되어 작업을 처리 1 connection 1 thread
논블로킹 : 스레드하나에 여러개의 커넥션이 연결되어 동시적으로 작업을 처리한다. N connection 1 thread 톰캣 8부터는 기본 이 설정

`server.tomcat.accept-count`

max-connections 이상의 요청이 들어왔을때 사용하는 요청 대기열 queue 사이즈, 기본값은 100
너무 크면 메모리를 많이 차지하고 너무 작으면 요청이 몰렸을때 들어오는 요청들을 거절해버릴 수 있다.
부적절하거나 잘못된 요청이 한번에 너무 많이 들어왔을때 서버에 장애를 발생시키는 것을 방지하는 목적도 있다.

