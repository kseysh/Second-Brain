```java
import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int commandsLength = commands.length;
        int[] answer = new int[commandsLength];
        
        for(int index = 0; index < commandsLength; index++){
            int i = commands[index][0];
            int j = commands[index][1];
            int k = commands[index][2];
        
            int[] arr = Arrays.copyOfRange(array, i - 1, j);
            Arrays.sort(arr);
            answer[index] = arr[k - 1];
            
        }
        
        return answer;
    }
}
```
Arrays.copyOfRange를 잘 알아둡시다.