```java
import java.io.*;
import java.util.*;

class Solution {
    public String[] solution(String[] players, String[] callings){
        Map<String, Integer> countMap = new HashMap<>();
        
        for(int i = 0; i < players.length; i++){
            countMap.put(players[i],i);
        }
        
        for(String playerName : callings){
            int playerRank = countMap.get(playerName);
            int frontPlayerRank = playerRank - 1;
            String frontPlayerName = players[playerRank - 1];
            
            countMap.replace(playerName, playerRank - 1);
            players[frontPlayerRank] = playerName;
            
            countMap.replace(frontPlayerName, frontPlayerRank + 1);
            players[playerRank] = frontPlayerName;
        }
        
        return players;
        
    }
}
```

## 배운 점
HashMap은 값으로 객체형태만 받는다
map.replace 메서드를 사용하면 값을 변경할 수 있다.