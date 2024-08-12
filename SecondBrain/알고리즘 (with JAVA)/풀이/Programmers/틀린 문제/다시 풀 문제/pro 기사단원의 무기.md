https://school.programmers.co.kr/learn/courses/30/lessons/136798
# 틀린 내 풀이
```java
class Solution {
    public int solution(int number, int limit, int power) {
        int[] powerArr = new int[number + 1];
        for(int i = 1; i <= number; i++){
            for(int j = 1; j <= number; j++){
                int checkNum = i * j;
                if(checkNum <= number && checkNum > 0){
                    powerArr[checkNum]++;
                }
            }
        }
        
        int answer = 0;
        for(int i = 1; i < powerArr.length; i++){
            int p = powerArr[i];
            if(p <= limit){
                answer += p;
            } else{
                answer += power;    
            }   
        }    
        return answer;
    }
}
```
### 틀린 이유
하나의 약수를 구하는데 O(n)이라는 오랜 시간이 걸립니다. 약수를 구하는 방식은 더 최적화를 진행할 수 있습니다.

# 다른 사람의 풀이

```java
import java.util.*;

class Solution {
    public int solution(int number, int limit, int power) {
        int answer = 0;
        
        for(int i=1;i<=number;i++){
            int cnt = 0;
            for(int j=1;j*j<=i;j++){
                if(j*j==i) cnt++;
                else if(i%j==0) cnt+=2;
            }
            
            if(cnt>limit) cnt = power;
            answer += cnt;
        }
        
        return answer;
    }
}
```

