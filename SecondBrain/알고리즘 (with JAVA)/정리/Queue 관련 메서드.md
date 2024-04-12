```java

Queue<Integer> queue = new LinkedList<>(); 

//큐에 요소 추가(enqueue) 
queue.add(1); // 문제 상황에서 예외 발생 
queue.offer(2); // 문제 상황에서 false 리턴 

//큐에서 요소 제거(dequeue) 
queue.remove(); // 문제 상황에서 예외 발생 
queue.pool(); // 문제 상황에서 null 리턴 

//큐 비우기 
queue.clear(); 

//큐의 최전방 요소 확인 
queue.element(); // 문제 상황에서 예외 발생 
queue.peek(); // 문제 상황에서 null 리턴
```