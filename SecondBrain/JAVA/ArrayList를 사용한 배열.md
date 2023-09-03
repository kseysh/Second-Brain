### ✅ 배열을 표현하는 방식은 크게 2가지

1. `[]`을 사용하는 방식**
    
    ```java
    int[] numbers = new int[2];
    numbers[0] = 1;
    numbers[1] = 2;
    ```
    
2. `ArrayList`를 사용하는 방식**
    
    ```java
    List<Integer> numbers = new ArrayList<>();
    numbers.add(1);
    numbers.add(2);
    numbers.add(3);
    ```
    

위 2가지 방식 중 1번째가 간단해보이는가? 안타깝게도 현업에서는 1번째 방식보다는 **2번째 방식을 사용**한다.

### ✅ `List`를 쓸 때 어떤 타입을 담을 건지 알려줘라.

무슨 말인지 실전 예시로 보자.

```java
// List에 문자형(String)을 담고 싶은 경우
List<String> names = new ArrayList<>();

// List에 정수형(int)을 담고 싶은 경우
List<Integer> ages = new ArrayList<>();

// List에 실수형(double)을 담고 싶은 경우
List<Double> weights = new ArrayList<>();

// List에 참거짓형(boolean)을 담고 싶은 경우
List<Boolean> trueOrFalse = new ArrayList<>();
```

### ✅ 이렇게도 쓰지 말자 1

아래와 같이 코드를 작성해도 잘 작동하는데 좋지 않은 코드이다. 왜 좋지 않은 코드인지는 제네릭(Generic)을 공부하면 알게 된다. 그러니 위에서 작성한 코드의 형태로 작성하는 습관을 들여라.

```java
List names = new ArrayList();
List ages = new ArrayList();
List weights = new ArrayList();
List trueOrFalse = new ArrayList();
```

### ✅ 이렇게도 쓰지 말자 2

```java
List<String> names = new ArrayList<**String**>();
```

`=`(등호)의 오른쪽에 있는 부분의 제네릭의 타입은 생략할 수 있다. 그러니 쓰지 않도록 하자.

### ✅ 이것만은 꼭 정리해놓자!

- `ArrayList`에 데이터 삽입 방법 (`add()`)
- `ArrayList`의 특정 데이터 조회 방법 (`get()`)
- `ArrayList`의 모든 데이터 조회 방법 (`get()`)
- `ArrayList`의 특정 데이터 삭제 방법 (`remove()`)
- `ArrayList`의 특정 데이터 변경 방법 (`set()`)
- `ArrayList`에 들어있는 특정 값의 인덱스 조회 방법 (`indexOf()`)
- `ArrayList` 내부에 있는 데이터를 오름차순, 내림차순으로 정렬하는 방법 (`Collections.sort()`)
- `ArrayList`에 특정 값이 존재하는 지 확인하는 방법 (`contains()`)
- `ArrayList`에 들어있는 데이터 총 개수 조회 방법 (`size()`)
- `ArrayList`에 들어있는 값 중 최대값, 최소값 조회 방법 (`Collecitons.max()`, `Collections.min()`)

### 왜 배열을 사용하지 않고 ArrayList를 사용해야 할까?
- 자바에서 배열은 정적배열 밖에 선언이 되지 않지만 ArrayList는 동적배열로 만들 수 있기 때문이다! 
- List interface를 ArrayList라는 구체클래스의 상위 타입으로 덧씌워서 캡슐화와 낮은 결합도의 원리를 충족하도록 한다.
- 이렇게 작성한다면 테스트 코드를 작성하기도 쉬워지고 나중에 구현에 변경사항이 생기더라도 변화에 쉽게 대응할 수 있다.
- 만약 나중에 ArrayList가 아니라 Stack의 기능으로 바꿔야한다면 ArrayList를 Stack 클래스로 바꾸어주기만 하면 되기 때문이다.

아래와 같은 클래스에서 위 리스트 list를 사용하는 코드가 있을때, 만약 List가 아닌 Stack으로 파라미터의 타입을 선언했다면

우리는 ArrayList 에서 Stack으로 list를 바꿀때, sizeOf() method를 호출하는 코드를 고쳐야만 합니다.

sizeOf() 메서드는 parameter의 타입에 의존적이기 때문입니다.

**List 로 한다면 Stack 이든, ArrayList 든 List 를 구현한 구체클래스이기만 한다면 코드를 안바꿔도 됩니다**

```java
class Counter {
    static int sizeOf(List<?> items) {
        return items.size();
    }
}

List<Object> list = new Stack<Object>();

int size = Counter.sizeOf(list);
```

이것을 우리는 결합도를 낮추고 의존성을 분리한다고 합니다. 한코드의 변화가 다른 코드의 변화를 만들어내지 않을수록 생산성이 높아지기 마련입니다.

> 이를 확장하게 되면 객체는 인터페이스를 사용해 선언하는 것이 좋다는 결론에도 도달할 수 있습니다.


출처
https://choichumji.tistory.com/165
https://jscode.notion.site/19ecb62fc710428c9b491d198dcbb69d