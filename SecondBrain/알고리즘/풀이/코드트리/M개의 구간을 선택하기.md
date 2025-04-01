# 나의 풀이
```cpp
#include <iostream>
#include <algorithm>
#define MAX_N 500
#define MAX_M 260
#define EMPTY -200'000'000
using namespace std;

int n,m;
int seq[MAX_N + 1];
int sum[MAX_N + 1];
int dp[MAX_N + 1][MAX_M];

int getSum(int sIdx, int eIdx){
    return sum[eIdx] - ((sIdx == 0) ? 0 : sum[sIdx - 1]);
}

int main() {
    cin >> n >> m;
    cin >> seq[0];
    sum[0] = seq[0];
    for(int i = 1; i < n; i++){
        cin >> seq[i];
        sum[i] = sum[i - 1] + seq[i]; // 구간 3 - 4의 합은 sum[4] - sum[2]
    }

    for(int i = 0; i < n; i++){
        for(int j = 0; j <= m; j++){
            dp[i][j] = EMPTY;
        }
    }

    for(int i = 0; i < n; i++){
        for(int l = 0; l <= i; l++){
            dp[i][1] = max(dp[i][1], getSum(l, i));
            if(i != 0) dp[i][1] = max(dp[i][1], dp[i - 1][1]);
        }
    }

    for(int i = 0; i < n; i++){
        for(int j = 2; j <= i; j++){
            for(int k = 1; k < m; k++){    
                if(dp[j-2][k] == EMPTY) continue;
                dp[i][k + 1] = max(dp[i][k + 1], dp[j - 2][k] + getSum(j, i));
                if(i != 0) dp[i][k + 1] = max(dp[i][k + 1], dp[i - 1][k + 1]);
            }
        }
    }    

    int ans = EMPTY;
    for(int i = 0; i < n; i++){
        ans = max(ans, dp[i][m]);
    }
    cout << ans;
    return 0;
}
```
참 오래도 풀었다...
`dp[i][k + 1] = max(dp[i][k + 1], dp[i - 1][k + 1]);`
부분을 이용해 k가 같을 때, i가 증가하더라도 계속 dp\[i]\[k]는 최댓값을 유지할 수 있도록 하였다.
