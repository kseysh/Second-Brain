```java
import java.util.*;

class Solution {
    public int[] solution(String[] keymap, String[] targets) {
        
        Map<Character, Integer> countMap = new HashMap<>();
        for(int i = 0 ; i < keymap.length; i++){
            for(int j = 0; j < keymap[i].length(); j++){
                char c = keymap[i].charAt(j);
                if(countMap.containsKey(c)){ // 키가 map에 등록되어 있다면 기존에 값과 비교한다.
                    int count = countMap.get(c);
                    if(count > j + 1){
                        countMap.replace(c, j + 1);
                    }
                    
                } else{ // 키가 map에 등록되어 있지 않다면 등록한다.
                    countMap.put(c, j + 1);  
                }
            }
        }
        
        int[] answer = new int[targets.length];
        for(int i = 0; i < targets.length; i++){
            for(int j =0; j < targets[i].length(); j++){
                char c = targets[i].charAt(j);
                if(!countMap.containsKey(c)){
                    answer[i] = -1;
                    break;
                }
                answer[i] += countMap.get(c);
            }
        }
        
        return answer;
    }
}
```

문제 엣지 케이스 잘 확인합시다.