```java
class Solution {
    public long[] solution(int x, int n) {
        long[] answer = new long[n];
        long previousValue = 0;
        for(int i = 0; i < n; i++){
            previousValue += x;
            answer[i] = previousValue;
        }
        
        return answer;
    }
}
```