# Queue 선언
```java
Queue<Integer> queue = new LinkedList<>();
```

## Queue에서 데이터 삭제
```java
queue.remove() // 큐가 비어있으면 예외 발생
queue.poll() // 큐가 비어있으면 null을 반환
queue.clear() // 큐의 모든 요소 삭제
```

## Queue에서 데이터 추가
```java
queue.add(x); // 큐가 꽉 찬 경우 에러 발생
queue.offer(x); // 값 추가 실패 시 false 반환
```

## Queue의 맨 앞 값 확인
```java
queue.element(); // 맨 앞 값이 비어있으면 에러 발생 
queue.peek(); // 맨 앞 값이 비어있으면 null 반환
```