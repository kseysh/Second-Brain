```java
import java.util.*;
class Solution {
    public int solution(int k, int m, int[] score) {
        Arrays.sort(score);

        int answer = 0;
        
        int index = score.length - 1;
        while(index > 0){
            if(index + 1 < m){
                return answer;
            }
            
            int min = Integer.MAX_VALUE;
            for(int i = 0; i < m; i++){ // box 처리
                if(min > score[index - i]){
                    min = score[index - i];   
                }
            }
            answer += (min * m);
            index -= m;
        }
        
        return answer;
    }
}
```

## 다른 사람의 풀이
```java
import java.util.*;

class Solution {
    public int solution(int k, int m, int[] score) {
        int answer = 0;

        Arrays.sort(score);

        for(int i = score.length; i >= m; i -= m){
            answer += score[i - m] * m;
        }

        return answer;
    }
}
```
생각해보니 이거... sorted 된 것이라 굳이 min 값을 찾지 않아도 되네요.