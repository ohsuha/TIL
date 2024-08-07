# garbage collector

> 가비지 컬렉터는 더 이상 참조되지 않고 사용하지 않는 객체들을 메모리에서 삭제한다.

- JVM은 heap 공간에 객체들을 관리한다.
- heap은 크게 Young, Old, Perm 영역으로 구분할 수 있다.
- Young 영역은 Eden, Survivor, Space, Virtual 영역으로 나뉘어진다.

## 자바에서 메모리가 살아가는 과정
1. Eden 영역에서 객체가 생성된다.
2. Eden 영역이 꽉 차면 살아있는 객체만 Survivor 영역으로 복사되고, 다시 Eden 영역을 채우게 된다.
3. Survivor 영역이 꽉 차게 되면 다른 Survivor 영역으로 객체가 복사된다. 이때, Eden 영역에 있는 객체들 중 살아있는 객체들도 다른 Survivor 영역으로 간다. 즉 Survivor 영역의 둘중 하나는 반드시 비어 있어야 한다. 이 부분을 `minor GC`, `young GC` 라고 부른다. 
4. 오래 살아있는 객체들은 Old 영역으로 이동한다. 지속적으로 이동하다가 Old 영역이 꽉 차면 garbage collection 이 발생하는데 이것을 `major GC`, `Full GC` 라고 부른다. 


크게 5가지의 GC 방식이 있지만 그 중 `SerialGC`는 WAS 로 사용하는 JVM에서 절대 사용해선 안된다. 이 방식은 클라이언트용 장비에 최적화된 것이기 때문에 만약 WAS에서 이 방식을 사용하면 속도가 매우 느려진다.