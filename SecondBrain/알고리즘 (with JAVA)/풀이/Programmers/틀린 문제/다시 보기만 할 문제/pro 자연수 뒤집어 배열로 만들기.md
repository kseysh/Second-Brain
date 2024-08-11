```java
class Solution {
    public int[] solution(long n) {
        int nLength = Long.toString(n).length();
        int[] answer = new int[nLength];
        int currentCount = 0;
        while(n != 0){
            int s = (int)(n%10);
            n/=10;
            answer[currentCount++] = s;
        }
        
        
        return answer;
    }
}
```


(int)n%10 은 n을 int로 바꾸고 %10을 하니 주의한다.
경계 조건을 테스트 케이스에 추가해본다.