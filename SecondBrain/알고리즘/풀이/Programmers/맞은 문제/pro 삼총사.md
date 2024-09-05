```java
class Solution {
    public int solution(int[] number) {
        int numberLength = number.length;
        
        int answer = 0;
        for(int firstIdx = 0; firstIdx < numberLength - 2; firstIdx++){
            for(int secondIdx = firstIdx + 1; secondIdx < numberLength - 1; secondIdx++){
                for(int thirdIdx = secondIdx + 1; thirdIdx < numberLength; thirdIdx++){
                    int first = number[firstIdx];
                    int second = number[secondIdx];
                    int third = number[thirdIdx];
                    
                    if(first + second + third == 0){
                        answer++;
                    }
                }   
            }    
        }
        
        
        return answer;
    }
}
```
문제를 잘 읽어보면 최적화를 전혀 진행하지 않아도 된다.