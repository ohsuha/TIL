# 노트북 ip주소 찾아보기
터미널 ifconfig | grep inet
# 노트북으로 띄운거 API서버 핸드폰으로 접근해보기
안되는데요 ㅠㅠㅠ
# 쿼리 실행 계획을 알려주는 explain 찾아보기
```sql
EXPLAIN SELECT * FROM orders WHERE customer_id = 1;
```
EXPLAIN 을 통해 쿼리가 어떻게 실행될지에 대한 정보를 받을 수 있다.
이를 통해 쿼리 최적화, 병목현상을 찾아낼 수 있다.

| 컬럼명         | 설명                                                                                           |
|----------------|------------------------------------------------------------------------------------------------|
| `id`           | 쿼리 내에서 실행 순서를 나타내는 식별자. 동일한 `id`는 동시에 실행되고, 다른 `id`는 순차적으로 실행됨.     |
| `select_type`  | SELECT의 유형을 나타냄. (예: `SIMPLE`, `PRIMARY`, `SUBQUERY`, `DERIVED`, `UNION`, `DEPENDENT SUBQUERY`) |
| `table`        | 쿼리가 참조하는 테이블 이름.                                                                       |
| `partitions`   | 쿼리가 테이블에서 접근하는 파티션을 나타냄. (MySQL 파티셔닝을 사용하는 경우)                                 |
| `type`         | 쿼리의 데이터 접근 방식. 성능에 영향을 미치며, 좋은 성능을 위해서는 `ALL`보다 인덱스를 사용하는 방식이 더 좋음. (예: `ALL`, `index`, `range`, `ref`, `eq_ref`, `const`, `system`) |
| `possible_keys`| 쿼리에서 사용할 수 있는 인덱스 목록.                                                             |
| `key`          | 실제로 사용된 인덱스. 인덱스를 사용하지 않았다면 `NULL`로 표시됨.                                   |
| `key_len`      | 사용된 인덱스의 길이. 인덱스에서 참조되는 바이트 수를 나타냄.                                            |
| `ref`          | 인덱스에서 비교되는 컬럼이나 상수를 나타냄.                                                             |
| `rows`         | 쿼리가 탐색해야 하는 행(row)의 예상 수. 더 적을수록 성능이 좋음.                                        |
| `filtered`     | WHERE 절에 의해 필터링된 행의 비율을 나타냄. 100%이면 모든 행이 필터에 의해 선택됨.                                |
| `Extra`        | 쿼리 실행에 대한 추가 정보. `Using where`, `Using index`, `Using temporary`, `Using filesort` 등의 정보가 포함됨. |

## EXPLAIN ANALYZE
-> Index lookup on c using carts_unique (user_id=1)  (cost=0.35 rows=1) (actual time=0.0258..0.0258 rows=0 loops=1)

실제 실행된 소요 시간, 비용을 측정하여 실행 계획 정보를 출력하고 싶다면 ANALYZE 키워드를 사용해서 정확하게 분석할 수 있습니다.

참조 : https://0soo.tistory.com/235

## java jpa 에서 실행하기
```java
@Service
public class QueryPlanService {

    private final EntityManager entityManager;

    public QueryPlanService(EntityManager entityManager) {
        this.entityManager = entityManager;
    }

    public List<Object[]> getQueryPlan(String sql) {
        String explainQuery = "EXPLAIN " + sql;
        Query query = entityManager.createNativeQuery(explainQuery);
        return query.getResultList();
    }
}
```

# Spring 환경에서 내부 함수의 성능은 어떻게 측정할 수 있을까요?
AOP 를 이용해 함수 실행 전후에 성능을 측정할 수 있다.
```java
@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* hello.hellospring..*(..))") // 패키지 하위에 모두 적용
    public Object execut(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        System.out.println("Start: " + joinPoint.toString());
        try {

            return joinPoint.proceed(); // 다음 로직으로 넘어간다.
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("End: " + joinPoint.toString() + " " + timeMs + "ms");
        }
    }
}
```
참조 : https://velog.io/@woply/Spring-AOP-%EA%B4%80%EC%A0%90%EC%9C%BC%EB%A1%9C-%EC%8B%A4%ED%96%89-%EC%86%8D%EB%8F%84-%EC%B8%A1%EC%A0%95%ED%95%98%EA%B8%B0

# 이렇게 측정한 것들은 어떻게 모니터링할 수 있을까요?
Spring AOP와 Micrometer를 결합하면, 메서드 성능 데이터를 Micrometer를 통해 수집하여 Prometheus 같은 시스템에 전송하고, Grafana 같은 도구를 통해 모니터링할 수 있습니다.

```java
implementation 'io.micrometer:micrometer-core'
implementation 'io.micrometer:micrometer-registry-prometheus'

@Aspect
@Component
public class PerformanceAspect {

	private final MeterRegistry meterRegistry;

	public PerformanceAspect(MeterRegistry meterRegistry) {
		this.meterRegistry = meterRegistry;
	}

	@Around("@annotation(performanceCheck)")
	public Object measureExecutionTime(ProceedingJoinPoint joinPoint, PerformanceCheck performanceCheck) throws Throwable {
		long start = System.currentTimeMillis();
		Object proceed = joinPoint.proceed();
		long executionTime = System.currentTimeMillis() - start;

		// 메서드 실행 시간을 Micrometer에 등록
		meterRegistry.timer("method.execution.time", "method", joinPoint.getSignature().getName())
			.record(executionTime, TimeUnit.MILLISECONDS);

		return proceed;
	}
}
```
Micrometer가 메서드 실행 시간을 기록하고, 이를 Prometheus나 다른 메트릭 시스템으로 전송할 수 있습니다.
Prometheus와 Grafana로 모니터링
Prometheus를 설정해 Spring Boot Actuator와 통신하게 하고 메트릭 데이터를 수집합니다.
Grafana를 통해 Prometheus에 저장된 데이터를 시각화할 수 있습니다.
Prometheus 설정 파일에 Spring 애플리케이션의 Actuator 엔드포인트를 추가하고, 데이터를 수집하도록 구성합니다.
