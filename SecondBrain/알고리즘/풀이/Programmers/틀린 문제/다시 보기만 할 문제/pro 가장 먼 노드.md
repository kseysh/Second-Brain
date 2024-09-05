```java
import java.util.*;

class Solution {

    public int solution(int n, int[][] edge) {

        ArrayList[] graph = new ArrayList[n + 1];
        int[] visitCounts = new int[n + 1];

        for(int i = 1; i < n + 1; i++){
            graph[i] = new ArrayList<Integer>();
        }

        for(int i = 0; i < edge.length; i++){
            graph[edge[i][0]].add(edge[i][1]);
            graph[edge[i][1]].add(edge[i][0]);
        }

        Queue<Integer> queue = new LinkedList<>();
        queue.add(1);
        int maxVisitCount = 0;
        while(!queue.isEmpty()){
            int currentNode = queue.poll();

            if(visitCounts[currentNode] > maxVisitCount){
                maxVisitCount = visitCounts[currentNode];
            }
            ArrayList<Integer> currentNodeList = graph[currentNode];
            for(int i = 0; i < currentNodeList.size(); i++){
                int visitNode = currentNodeList.get(i);
                if(visitCounts[visitNode] == 0){
                    visitCounts[visitNode] = visitCounts[currentNode] + 1;
                    queue.add(visitNode);
                }
            }
        }

        int answer = 0;
        for(int i = 2; i <= n; i++){
            if(visitCounts[i] == maxVisitCount){
                answer++;
            }
        }

        return answer;
    }
}
```

# 틀린 이유
for문의 i 시작 지점을 잘못 잡고 있었다.
