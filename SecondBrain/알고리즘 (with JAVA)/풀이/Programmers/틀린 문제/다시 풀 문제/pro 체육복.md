```java
import java.util.*;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        final int RESERVED = -100;
        
        int answer = n - lost.length;
        Arrays.sort(lost);
        Arrays.sort(reserve);
        
        for(int i =0; i < lost.length; i++){ // 체육복을 가지고 있는데, 잃어버린 사람들
            int lostNumber = lost[i];
            for(int j = 0; j < reserve.length; j++){
                int reserveNumber = reserve[j];
                if(lostNumber == reserveNumber){
                    lost[i] = RESERVED;
                    reserve[j] = RESERVED;
                    answer++;
                    break;
                }
            }    
        }
        
        for(int i =0; i < lost.length; i++){
            int lostNumber = lost[i];
            for(int j = 0; j < reserve.length; j++){
                int reserveNumber = reserve[j];
                if(lostNumber - 1 == reserveNumber || lostNumber + 1 == reserveNumber){
                    lost[i] = RESERVED;
                    reserve[j] = RESERVED;
                    answer++;
                    break;
                }
            }    
        }
        
        return answer;
    }
}
```
조건을 무시해도 된다 생각했더니...