```java
import java.util.*;

class Solution {
    public int[] solution(String[] name, int[] yearning, String[][] photo) {
        Map<String, Integer> rememberCount = new HashMap<>();
        int[] answer = new int[photo.length];
        
        for(int i = 0; i < name.length; i++){
            rememberCount.put(name[i], yearning[i]);
        }
        
        for(int i = 0; i < photo.length; i++){
            int result = 0;
            for(String n : photo[i]){
                if(rememberCount.containsKey(n)){
                    result += rememberCount.get(n);    
                }
            }
            answer[i] = result;
        }
        
        return answer;
    }
}
```

map.get 함수를 사용하는 과정에서 NPE 발생하였습니다.
문제를 잘 읽읍시다.