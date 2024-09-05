# 나의 풀이
```java
import java.util.*;
class Solution {
    ArrayList<Integer>[] graph;
    int[] childCounts;
    boolean[] isVisited;
    
    public int solution(int n, int[][] wires) {
        isVisited = new boolean[n + 1];
        childCounts = new int[n + 1];
        graph = new ArrayList[n + 1];
        
        for(int i = 0; i <= n; i++){
            graph[i] = new ArrayList<>();
        }
        
        for(int i = 0; i < wires.length; i++){
            graph[wires[i][0]].add(wires[i][1]);
            graph[wires[i][1]].add(wires[i][0]);
        }
        
        postOrder(1);
                
        return getDifferenceOfUnlinkedWireCount(n);
    }
    
    public int postOrder(int currentWire){
        isVisited[currentWire] = true;
        int childCount = 1;
        for(int wireNumber : graph[currentWire]){
            if(!isVisited[wireNumber]){
                childCount += postOrder(wireNumber);
            }
        }
        childCounts[currentWire] = childCount;
        return childCount;
    }
    
    public int getDifferenceOfUnlinkedWireCount(int n){
        int result = 1000;
        for(int childCount : childCounts){
            if(childCount*2 > n){
                result = Math.min(result, childCount * 2 - n);
            }else{
                result = Math.min(result, n - childCount * 2);
            }
        }
        return result;
    }
}
```

# 다른 사람의 풀이
```java
class Solution {
    int N, min;
    int[][] map;
    int[] vst;
    int dfs(int n){
        vst[n] = 1;
        int child = 1;
        for(int i = 1; i <= N; i++) {
            if(vst[i] == 0 && map[n][i] == 1) {
                child += dfs(i);
            }
        }
        min = Math.min(min, Math.abs(child - (N - child)));
        return child;
    }
    public int solution(int n, int[][] wires) {
        N = n;
        min = n;
        map = new int[n+1][n+1];
        vst = new int[n+1];
        for(int[] wire : wires) {
            int a = wire[0], b = wire[1];
            map[a][b] = map[b][a] = 1;
        }
        dfs(1);
        return min;
    }
}
```