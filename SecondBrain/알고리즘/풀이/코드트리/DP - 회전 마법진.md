한 방향으로 돌리는게 최적이다!

```cpp
#include <iostream>
#include <climits>
#define MAX_N 10000
using namespace std;

int dp[MAX_N + 1][11];
int n;
string cur, target;

int main() {
    cin >> n >> cur >> target;

    for(int i = 0; i <= n; i++){
        for(int j = 0; j < 10; j++){
            dp[i][j] = INT_MAX;
        }
    }
    dp[0][0] = 0;

    for(int i = 0; i <= n; i++){
        for(int j = 0; j < 10; j++){
            if(dp[i][j] == INT_MAX) continue;

            int cv = (cur[i] - '0' + j) % 10;
            int tv = target[i] - '0';

            // 반 시계 방향 회전
            int cost = (tv - cv + 10) % 10;
            int nj = (j + cost) % 10;
            dp[i + 1][nj] = min(dp[i + 1][nj], dp[i][j] + cost);

            // 시계 방향 회전
            cost = (cv - tv + 10) % 10;
            dp[i + 1][j] = min(dp[i + 1][j], dp[i][j] + cost);
        }
    }

    int ans = INT_MAX;
    for(int i = 0; i < 10; i++){
        ans = min(ans, dp[n][i]);
    }
    cout << ans;

    return 0;
}
```
