```java
import java.util.*;

class Solution {
    ArrayList<Integer>[] winGraph;
    ArrayList<Integer>[] loseGraph;
    boolean[] isVisited;
    
    public int solution(int n, int[][] results) {
        winGraph = new ArrayList[n + 1];
        loseGraph = new ArrayList[n + 1];
        
        for(int i = 1; i <= n; i++){
            winGraph[i] = new ArrayList<Integer>();
            loseGraph[i] = new ArrayList<Integer>();
        }
        
        for(int i = 0; i < results.length; i++){
            winGraph[results[i][0]].add(results[i][1]);
            loseGraph[results[i][1]].add(results[i][0]);
        }
        
        int answer = 0;
        for(int i = 1; i <= n; i++){
            isVisited = new boolean[n + 1];
            int winCount = winDfs(i, 0) - 1;
            isVisited = new boolean[n + 1];
            int loseCount = loseDfs(i, 0) - 1;
            
            if(winCount + loseCount == n - 1){
                answer++;
            }
        }
        
        return answer;
    }
    
    public int winDfs(int currentPlayerNumber, int currentCount){
        isVisited[currentPlayerNumber] = true;

        int childCountSum = 0;

        for(int loserNumber : winGraph[currentPlayerNumber]){
            if(!isVisited[loserNumber]){
                childCountSum += winDfs(loserNumber, currentCount);
            }
        }
        return currentCount + childCountSum + 1;
    }
    
    public int loseDfs(int currentPlayerNumber, int currentCount){
        isVisited[currentPlayerNumber] = true;
        
        int childCountSum = 0;
        for(int winnerNumber : loseGraph[currentPlayerNumber]){
            if(!isVisited[winnerNumber]){
                childCountSum += loseDfs(winnerNumber, currentCount);
            }
        }
        return currentCount + childCountSum + 1;
    }
}
```