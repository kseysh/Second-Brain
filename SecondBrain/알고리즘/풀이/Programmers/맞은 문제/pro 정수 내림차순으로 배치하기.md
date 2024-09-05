```java
class Solution {
    public long solution(long n) {
        int nLength = Long.toString(n).length();
        int[] numberCount = new int[10];
        
        while(n != 0){
            int mod = (int)(n%10);
            n /= 10;
            numberCount[mod]++;
        }
        
        StringBuilder sb = new StringBuilder();
        
        for(int i = 9; i >= 0 ; i--){
            for(int j = 0; j < numberCount[i]; j++){
                sb.append(i);
            }
        }
        
        
        long answer = Long.parseLong(sb.toString());
        return answer;
    }
}
```