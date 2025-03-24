## 나의 풀이
```cpp
#include <iostream>
#define MAX_N 100
#define MAX_M 10000
#define EMPTY 200000
using namespace std;

int n, m;
int seq[MAX_N];
int dp[MAX_M+1];
int temp[MAX_M+1];

int main() {
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> seq[i];
    }

    for(int i = 1; i <= m; i++){
        dp[i] = EMPTY;
        temp[i] = EMPTY;
    }

    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(dp[j] == EMPTY) continue;
            if((seq[i] + j) > m) continue;
            temp[seq[i] + j] = min(dp[seq[i] + j], dp[j] + 1);
        }

        for(int j = 0; j <= m; j++){
            dp[j] = temp[j];
        }
    }

    cout << ((dp[m] == EMPTY) ? -1 : dp[m]);

    return 0;
}
```
temp를 사용해 동전을 한 번만 사용하도록 유도했다

## 정석 풀이
![[Pasted image 20250324115615.png|500]]
풀이를 보면 뒤에서부터 포문을 통해 dp를 채우게 되면 동전을 최대 1번씩만 써서 만들 수 있는 최소 동전의 수를 구하는 dp가 될 수 있다.