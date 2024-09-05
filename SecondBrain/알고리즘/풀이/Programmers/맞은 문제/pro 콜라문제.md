```java
class Solution {
    public int solution(int a, int b, int n) {
        
        int answer = 0;
        while(n >= a){
            int earnedBottleCount = ( n / a ) * b;
            answer += earnedBottleCount;
            n = n % a + earnedBottleCount;
        }
        return answer;
    }
}
```
ez