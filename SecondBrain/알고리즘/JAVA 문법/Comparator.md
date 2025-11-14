
```java
req.sort(Comparator.comparingInt(r -> r.waitingTime));
```

이 방식을 꼭 알아두자.
```java
PriorityQueue<Request> requestReadyQueue = new PriorityQueue<>((r1, r2) -> r2.boardingCount - r1.boardingCount);
```