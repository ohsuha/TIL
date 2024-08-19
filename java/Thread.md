# Thread
> 프로세스 내에서 실행되는 개별적인 작업 단위. 프로세스와 같은 메모리 공간을 공유하므로, 같은 프로세스 내에서 데이터와 자원을 공유할 수 있다.
 
- JVM은 적어도 32~64 MB의 메모리를 점유하지만 스레드는 1MB이내의 메모리를 점유한다. 경량 프로세스 라고도 한다.
- 스레드가 어떤 작업을 할지 구현하는 메소드는 run() 이다. run() 메소드가 종료되면 새로 생성된 스레드도 종료된다.
- 스레드를 시작하게 하는 메소드는 start() 이다.
- start() 메소드는 자바에서 run() 메소드를 수행하도록 되어있다. 프로세스가 아닌 하나의 스레드를 JVM에 추가하여 실행시킨다.
- 스레드에 매개변수를 전달하고자 할 때는 생성자 매개 변수를 통해 전달할 수 있다.
- 스레드 종료는 반드시 interrupte() 를 사용, stop() 을 사용하지 말자

## 스레드의 진행 상태
![img](https://joont92.github.io/temp/thread-states.png)

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

## 왜 두가지 방식을 제공할까?
- extends 는 다중상속이 불가능하므로 달느 클래스를 상속 중일 경우 Thread 를 상속할 수 없기 떄문에, 상황에 따라 적절히 사용하자.

## 데몬스레드
데몬 스레드는 백그라운드에서 실행되며, 프로그램의 주요 작업과는 별도로 실행되는 스레드. 프로그램이 종료되면 자동으로 종료된다.
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

## synchronized
> 멀티스레드 환경에서 공유 자원의 동기화를 보장하기 위해 사용되는 키워드

- 메소드에서 인스턴스 변수를 수정하려고 할 때 여러 스레드가 한 객체에 선언된 메소드에 접근하여 데이터를 처리하려고 할 때 동시에 연산을 수행하여 값이 꼬일 수 있다.
- synchronized는 동기화를 제공하지만, 너무 많은 동기화가 성능을 저하시킬 수 있다. 적절한 범위에서 사용해야한다. 필요한 부분만 동기화하고 나머지는 비동기적으로 처리하는 게 좋다.


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
