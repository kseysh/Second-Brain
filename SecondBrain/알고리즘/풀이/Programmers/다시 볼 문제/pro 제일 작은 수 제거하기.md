https://school.programmers.co.kr/learn/courses/30/lessons/12935#
```java
class Solution {
    public int[] solution(int[] arr) {
        int arrLength = arr.length;
        
        if(arrLength <= 1){
            int[] answer = {-1};
            return answer;
        }
        
        int min = Integer.MAX_VALUE;
        int minIdx = -1;
        
        for(int i = 0; i < arrLength; i++){
            if(min >= arr[i]){
                min = arr[i];
                minIdx = i;
            }
        }
        
        int[] answer = new int[arrLength-1];
        boolean flag = true;
        for(int i = 0; i < arrLength; i++){
            if(i != minIdx){
                if(flag){
                    answer[i] = arr[i];    
                }else{
                    answer[i - 1] = arr[i];
                }
                
            }else{
                flag = false;
            }
        }
        
        return answer;
    }
}
```
테스트 케이스 조건 경계를 맨뒤 중간 맨 앞 이렇게 세 개 정도는 부여해주자.

# 다른 사람의 풀이
```java 
import java.util.Arrays;
import java.util.stream.Stream;
import java.util.List;
import java.util.ArrayList;

class Solution {
  public int[] solution(int[] arr) {
      if (arr.length <= 1) return new int[]{ -1 };
      int min = Arrays.stream(arr).min().getAsInt();
      return Arrays.stream(arr).filter(i -> i != min).toArray();
  }
}
```
조건에 `- 인덱스 i, j에 대해 i ≠ j이면 arr[i] ≠ arr[j] 입니다.`가 있으므로 인덱스를 굳이 찾을 필요가 없었다.