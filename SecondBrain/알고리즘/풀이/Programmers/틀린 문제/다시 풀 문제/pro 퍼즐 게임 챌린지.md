# 나의 풀이
이분 탐색에서 (mid + 1, max) (min, mid - 1)을 몰라서 틀림..
```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <climits>
#define MAX_N 300000
#define MAX_DIFF 100000
using namespace std;

int n;
int answer = INT_MAX;
long long globalLimit;
vector<int> globalDiffs;
vector<int> globalTimes;

long long getPuzzleTime(int myLevel){
    long long useTime = 0;
    for(int i = 0; i < n; i++){
        if(globalDiffs[i] > myLevel){
            useTime += (globalTimes[i] + globalTimes[i-1]) * (globalDiffs[i] - myLevel);  
        }
        useTime += globalTimes[i];
    }
    return useTime;
}

void checkLevelRange(int s, int e){
    if(s > e){
        return;
    }
    int myLevel = (s + e)/2;
    long long useTime = getPuzzleTime(myLevel);
    if(useTime > globalLimit){ // 시간 초과
        checkLevelRange(myLevel + 1, e); // 내 숙련도 올림
    }else{// 시간 안에 들어옴
        answer = min(answer, myLevel); // 최솟값이라면 갱신
        checkLevelRange(s, myLevel - 1); // 내 숙련도 감소
        
    }
}

int solution(vector<int> diffs, vector<int> times, long long limit) {
    n = (int)diffs.size();
    globalTimes = times;
    globalDiffs = diffs;
    globalLimit = limit;
    checkLevelRange(1, MAX_DIFF);
    return answer;
}
```

## 다른 사람의 풀이
이분 탐색은 굳이 재귀가 아닌, while문으로 풀어도 되는 문제이다.
max_element 함수도 알아가자.
```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

int solution(vector<int> diffs, vector<int> times, long long limit) {
    
    int minLevel = 1;
    int maxLevel = *max_element(diffs.begin(), diffs.end());
    
    int answer = maxLevel;
    while (minLevel <= maxLevel)
    {
        int curLevel = (minLevel + maxLevel) / 2;
        
        long long time = 0;
        for (int i = 0; i < times.size(); i++)
        {
            int prev = i == 0 ? 1 : times[i-1];
            time += max(diffs[i]-curLevel, 0) * (times[i] + prev) + times[i];
        }
        
        if (time <= limit)
        {
            maxLevel = curLevel - 1;
            answer = min(answer, curLevel);
        }
        else
            minLevel = curLevel + 1;
    }
    
    return answer;
}
```