# Random.class
`java.util.Random` 클래스는 자바에서 난수를 생성하는 데 사용되는 클래스임. 이 클래스는 다양한 타입의 난수를 생성할 수 있게 도와줌. 예를 들어, 정수, 부동소수점 숫자, boolean 값 등을 무작위로 생성할 수 있음.

`Random` 클래스는 내부적으로 **시드(seed)**라는 값을 기반으로 난수를 생성함. 이 시드는 난수 생성 알고리즘의 시작점을 의미함. 같은 시드 값이 주어지면, `Random` 클래스는 동일한 난수 시퀀스를 생성하게 됨.

따라서, 특정 시드를 사용하면 난수 생성의 예측 가능성을 높일 수 있음. 예를 들어, 테스트 환경에서 반복 가능한 결과를 얻고 싶을 때 시드를 고정하여 사용함.

시드를 설정하지 않고 `Random` 객체를 생성하면, `Random` 클래스는 현재 시간(밀리초 단위)을 시드로 사용함. 이는 일반적으로 동일한 시퀀스가 반복되지 않도록 하기 위함임.

예를 들어:

```java
Random random = new Random(); // 시드를 지정하지 않으면, 현재 시간을 기반으로 시드가 설정됨
int randomNumber = random.nextInt(100); // 0부터 99 사이의 정수를 무작위로 생성
```

또는 특정 시드를 사용하여 `Random` 객체를 생성할 수도 있음:

```java
Random randomWithSeed = new Random(42); // 시드로 42를 사용
int randomNumber = randomWithSeed.nextInt(100); // 동일한 시드로 동일한 결과가 생성됨
```

이렇게 특정 시드를 사용하면 같은 시드로 생성된 `Random` 객체들은 같은 난수 시퀀스를 생성함. 이를 통해 난수 생성의 결과를 예측하거나 재현할 수 있음.

# Random 클래스 주요 메소드

| 메소드 | 설명 | 예시 코드 | 예시 결과 |
|--------|------|-----------|------------|
| `nextInt()` | 0을 포함하여, 지정된 범위 내의 임의의 정수를 반환함. | `random.nextInt(100);` | 0부터 99 사이의 정수 (예: 42) |
| `nextInt(int bound)` | 0부터 `bound-1`까지의 임의의 정수를 반환함. | `random.nextInt(50);` | 0부터 49 사이의 정수 (예: 25) |
| `nextLong()` | 임의의 `long` 값을 반환함. | `random.nextLong();` | 아주 큰 음수 또는 양수 (예: -1234567890123456789) |
| `nextDouble()` | 0.0(포함)과 1.0(제외) 사이의 임의의 `double` 값을 반환함. | `random.nextDouble();` | 0.0과 1.0 사이의 실수 (예: 0.723450123) |
| `nextFloat()` | 0.0(포함)과 1.0(제외) 사이의 임의의 `float` 값을 반환함. | `random.nextFloat();` | 0.0과 1.0 사이의 실수 (예: 0.34567f) |
| `nextBoolean()` | 임의의 `boolean` 값을 반환함. | `random.nextBoolean();` | `true` 또는 `false` (예: true) |
| `nextBytes(byte[] bytes)` | 임의의 바이트 배열을 채움. | `byte[] bytes = new byte[4]; random.nextBytes(bytes);` | 예: `[12, -45, 67, 89]` |
| `setSeed(long seed)` | 시드를 설정하여 난수의 예측 가능성을 높임. | `random.setSeed(12345L);` | 동일한 시드로 같은 결과 생성 |
