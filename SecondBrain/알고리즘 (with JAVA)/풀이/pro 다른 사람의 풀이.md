```java
class Solution {
    public int solution(int n) {
        boolean[] isNotPrime = new boolean[n + 1];
        
        if(n < 4){
            return 0;
        }
        int primeCount = 2;
        for(int i = 2; i <= Math.sqrt(n); i++){
            if(isNotPrime[i] == true){
                continue; // 소수가 아니라면 에라토스테네스의 체를 하지 않음
            }
            for(int j = 2; j <= n; j++){
                int notPrimeValue = i * j;
                if(notPrimeValue > n){
                    continue; // 찾을 값이 n보다 크면 넘김
                }
                if(!isNotPrime[notPrimeValue]){ // 소수 -> 합성수
                    isNotPrime[notPrimeValue] = true;
                }
            }
        }
        
        int answer = 0;
        
        for(int i = 4; i <= n; i++){
            if(isNotPrime[i]){
                answer++;
            }
        }
        
        return answer;
    }
}
```