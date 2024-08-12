https://school.programmers.co.kr/learn/courses/30/lessons/136798
# 틀린 내 풀이
```java
class Solution {
    public int solution(int number, int limit, int power) {
        int[] powerArr = new int[number + 1];
        for(int i = 1; i <= number; i++){
            for(int j = 1; j <= number; j++){ // j <= number / i로 하면 통과합니다.
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
class Solution {
    public int solution(int number, int limit, int power) {
        int answer = 0;
        
        for(int i = 1; i <= number; i++){
            int count = 0;
            
            for(int j = 1; j * j <= i; j++){
                if(j*j == i){
                    count++;        
                } else if(i%j == 0){
                    count+=2;
                }
            }
            
            if(count > limit){
                count = power;
            }
            answer += count;
        }
        
        return answer;
    }
}
```
### 최적화 한 내용
1. 약수를 제곱수인 수와 제곱수이지 않은 수로 나눈다.
2. 약수가 제곱수라면 count를 1 증가시키고, 제곱수가 아니라면 count를 2 증가시킨다.
3. 따로 powerArr을 사용하지 않고 바로 answer에 값을 더해주었다.