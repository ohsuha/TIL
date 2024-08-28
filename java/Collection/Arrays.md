Arrays 클래스에는 배열을 다루는데 유용한 메서드가 정의되어 있다.

## 배열의 복사 copyOf(), copyOfRange()
- copyOf()는 배열 전체를, copyOfRange()는 배열의 일부를 복사해서 새로운 배열을 만들어 반환한다.
```java
int[] original = {1, 2, 3, 4, 5};

int[] copy = Arrays.copyOf(original, original.length);
int[] partialCopy = Arrays.copyOfRange(original, 1, 4); // [2, 3, 4]
```
## 배열 채우기 fill(), setAll()
- fill()은 배열의 모든 요소를 지정된 값으로 채운다.
```java
int[] array = new int[5];
Arrays.fill(array, 42);
```
- setAll()은 배열을 채우는데 사용할 함수형 인터페이스를 매개변수로 받는다.
```java
int[] array = new int[5];
Arrays.setAll(array, i -> i * i);
```

## 배열의 정렬과 검색 sort(), binarySearch()
- sort()는 배열을 정렬할 때 사용한다.
```java
int[] array = {5, 2, 9, 1, 5, 6};
Arrays.sort(array);
```
- binarySearch()는 배열에서 지정된 값이 저장된 위치를 찾아서 반환하는데, 반드시 배열이 정렬된 상태여야 올바른 결과를 얻는다.
  - 이진검색을 통해 검색하려면 정렬이 되어있어야한다.
- 만일 검색한 갑소가 일치하는 요소들이 여러개 있다면 어떤 것의 위치가 반환될지는 알 수 없다.
```java
int[] sortedArray = {1, 2, 5, 5, 6, 9};
int index = Arrays.binarySearch(sortedArray, 5);
```

## 배열의 비교와 출력 equals(), toString()
- 다차원 배열에서는 deepEquals(), deepToString()을 사용해야한다.
```java
int[] array1 = {1, 2, 3};
int[] array2 = {1, 2, 3};
System.out.println(Arrays.equals(array1, array2)); // true
```
## 배열을 List로 반환 asList(Object... o)
- 배열을 List 에 담아서 반환한다. asList() 가 반환한 List 의 크기를 변경할 수 없다.
```java
String[] array = {"a", "b", "c"};
List<String> list = Arrays.asList(array);
System.out.println(list); // [a, b, c]
// list.add("d"); // UnsupportedOperationException
```
- 추가 또는 삭제가 불가능하다. 크기 변경이 필요하다면 다음과 같이 ArrayList 로 생성하자
```java
String[] array = {"a", "b", "c"};
List<String> list = new ArrayList<>(Arrays.asList(array));
list.add("d");
System.out.println(list); // [a, b, c, d]
```
## parallelXXX(), spliterator(), straem()
- parallelXXX 여러 스레드가 작업을 나누어 처리하도록 한다.
```java
int[] array = {1, 2, 3, 4, 5};
Arrays.parallelSort(array);
System.out.println(Arrays.toString(array)); // [1, 2, 3, 4, 5]
```
- spliterator() 는 여러 스레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누어준다.
```java
int[] array = {1, 2, 3, 4, 5};
Spliterator.OfInt spliterator = Arrays.spliterator(array);
spliterator.forEachRemaining(System.out::println);
```
- stream() 은 컬렉션을 stream 으로 변환한다.
```java
int[] array = {1, 2, 3, 4, 5};
Arrays.stream(array).forEach(System.out::println);
```