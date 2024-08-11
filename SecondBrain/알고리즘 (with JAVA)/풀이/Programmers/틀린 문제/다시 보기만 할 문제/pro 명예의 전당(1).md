```java
import java.util.PriorityQueue;

class Solution {
    public int[] solution(int k, int[] score) {
        int[] answer = new int[score.length];
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for(int i = 0 ; i < score.length; i++){
            int s = score[i];
            pq.add(s);
            if(pq.size() > k){
                pq.poll();
            }
            answer[i] = pq.peek();
        }
        
        return answer;
    }
}
```

priorityQueue의 사용법을 다시 알아봅시다.
메서드는 Queue와 같아요.
기본일시에는 오름차순으로 정렬되며, 아니라면 `PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());`로 선언해주면 됩니다.