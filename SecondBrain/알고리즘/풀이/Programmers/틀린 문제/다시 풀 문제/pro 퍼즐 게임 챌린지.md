# 나의 풀이
이분 탐색에서 
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