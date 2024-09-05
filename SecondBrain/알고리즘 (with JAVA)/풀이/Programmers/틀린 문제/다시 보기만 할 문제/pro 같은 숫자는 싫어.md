```java
import java.util.*;

public class Solution {
    public int[] solution(int []arr) {
        List<Integer> answerList = new ArrayList<>();
        int previous = -1;
        
        for(int i = 0; i < arr.length; i++){
            int current = arr[i];
            if(previous == current){
                continue;
            }
            answerList.add(current);
            previous = current;
        }
        
        return answerList.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

List to Array는 아래와 같이 사용한다.
```java
list.stream().mapToInt(Integer::intValue).toArray();
```

```java
for(int i=0; i<answer.length; i++) {
	answer[i] = list.get(i).intValue();
}
```