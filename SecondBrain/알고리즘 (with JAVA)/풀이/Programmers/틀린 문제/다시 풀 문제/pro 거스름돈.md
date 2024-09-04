```java
import java.util.*;
class Solution {
    public int solution(int n, int[] money) {
        Arrays.sort(money);
        
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for(int i = 0; i < money.length; i++){
            for(int j = money[i]; j <= n; j++){
                dp[j] += dp[j - money[i]];
            }
        }
        
        return dp[n];
    }
}
```

# 틀린 이유
dp에 대해서 잘 몰랐음 좀 더 공부해야 할 듯.