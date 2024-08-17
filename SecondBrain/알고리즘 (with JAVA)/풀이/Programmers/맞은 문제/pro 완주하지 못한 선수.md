```java
import java.util.*;
class Solution {
    public String solution(String[] participant, String[] completion) {
        Arrays.sort(participant);
        Arrays.sort(completion);
        
        for(int i = 0; i < completion.length; i++){
            if(!participant[i].equals(completion[i])){
                return participant[i];
            }
        }
        
        return participant[participant.length - 1];
    }
}
```

# 다른 사람의 풀이
```java
import java.util.HashMap;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        HashMap<String, Integer> hm = new HashMap<>();
        for (String player : participant) hm.put(player, hm.getOrDefault(player, 0) + 1);
        for (String player : completion) hm.put(player, hm.get(player) - 1);

        for (String key : hm.keySet()) {
            if (hm.get(key) != 0){
                answer = key;
            }
        }
        return answer;
    }
}
```
정석은 hashMap을 사용하여 풀이하는 듯 하다