## 풀이 보고 푼 풀이
```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

char c[17] = {'0','1','2','3','4','5','6','7','8','9',
             'A','B','C','D','E','F'};

string numToN(int num, int n){
    string temp = "";
    if(num == 0){
        return "0";
    }else{
        while(num != 0){
            temp += c[num % n];
            num /= n;
        }
    }
    reverse(temp.begin(), temp.end());
    return temp;
}

string solution(int n, int t, int m, int p) {
    
    string temp = "";
    
    int mt = m * t;
    for(int num = 0; num <= mt; num++){
        temp += numToN(num, n);
    }
    
    string answer = "";
    for(int i = 0; i < t; i++){
        answer += temp[p + m * i - 1];
    }
    
    return answer;
}
```
## 배운 지식
진법 변환하는 방법
algorithm 헤더의 reverse 함수
string은 `vector<char>`와 비슷하게 사용할 수 있다.

## 다시 푼 풀이
```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string s = "0";

string convertNum(int n, int count){
    string rcs =  "";
    char c;
    while(count != 0){
        int remain = count % n;
        count /= n;
        c = (remain >= 10) ? remain - 10 + 'A' : remain + '0';
        rcs += c;
    }
    reverse(rcs.begin(), rcs.end());
    return rcs;
}

string solution(int n, int t, int m, int p) {
    
    int count = 1;
    while(s.size() <= 110'000){
        s += convertNum(n, count++);
    }
        
    string answer = "";
    for(int i = 0; i < t; i++){
        answer += s[i * m + p - 1];
    }
    
    return answer;
}
```
4개월만에 다시 푼 풀이 
- 진법 변환시 char를 사용하는 방식 좋은 것 같음
- m * t 번만 진행해도 된다는 사실 좋은 것 같음