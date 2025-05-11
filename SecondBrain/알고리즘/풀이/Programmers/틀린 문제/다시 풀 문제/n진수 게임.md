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
