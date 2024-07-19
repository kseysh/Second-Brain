```java
class Solution {
    public int solution(int left, int right) {

        int answer = 0;
        for(int i = 0; i < right - left + 1; i++){
            int value = left + i;
            if(Math.sqrt(value) % 1 == 0){
                answer -= value;
            }else{
                answer += value;
            }
        }
        return answer;
    }
}
```