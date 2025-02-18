# 나의 풀이
```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <cmath>
using namespace std;
#define MAX_N 10
int n, allSum;
int ans = INT_MAX;
int nums[MAX_N * 2];
vector<int> v;
bool canUse[MAX_N * 2];

int getDiff(){
    int sum = 0;
    for(int value : v){
        sum += nums[value];
    }
    return abs(allSum - 2 * sum);
}

void simulate(int count){
    if(count == n){
        int diff = getDiff();
        ans = min(ans, diff);
        return;
    }

    for(int i = 0; i < 2 * n; i++){
        if(count > 0){
            if(i <= v[count - 1]) continue;
        }
        v.push_back(i);
        simulate(count + 1);
        v.pop_back();
    }
}

void init(){
    for(int i = 0; i < 2 * n; i++){
        canUse[i] = true;
        allSum += nums[i];
    }
}

int main() {
    cin >> n;
    for(int i = 0; i < 2 * n; i++){
        cin >> nums[i];
    }    
    init();
    simulate(0);
    cout << ans;
    return 0;
}
```
## 틀린 이유
백트래킹은 순열도 있지만 조합으로 푸는 문제도 있다.
따라서 순서를 신경쓰지 않아도 되는데 신경쓴다면, n의 시간 복잡도로 풀 수 있는 문제를 n!로 풀어야 하는 대참사가 발생한다.
따라서 백트래킹으로 풀 때는 주어진 문제가 어떻게 풀어야 하는 문제인지 잘 확인하고 풀도록 하자.