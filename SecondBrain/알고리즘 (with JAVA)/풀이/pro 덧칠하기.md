```java
class Solution {
    public int solution(int n, int m, int[] section) {
    
        int painted = 0;
        int answer = 0;
        for(int i = 0; i < section.length; i++){
            if(painted >= section[i]){ // 이미 페인팅 된 벽
                continue;
            } else {
                painted = section[i] + m - 1;
                answer++;
            }
        }
        return answer;
    }
}
```