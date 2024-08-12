# 틀린 풀이
```java
class Solution {
    public int solution(String[] babbling) {
        
        int answer = 0;
        for(String babblingWord : babbling){
            if(babblingWord.contains("yeye")){
                continue;
            }
            if(babblingWord.contains("woowoo")){
                continue;
            }
            if(babblingWord.contains("mama")){
                continue;
            }
            if(babblingWord.contains("ayaaya")){
                continue;
            }
            
            String replacedWord = babblingWord
                .replace("woo","")
                .replace("ma","")
                .replace("aya","")
                .replace("ye","");

            if(replacedWord.length() == 0){
                answer++;
            }
        }
        
        return answer;
    }
}
```
replace를 하여 `["yayae"]`가 주어졌을 때 aya가 먼저 replace되고, ye가 replace되어 반례가 있던 문제입니다.
# 수정한 풀이
```java
class Solution {
    public int solution(String[] babbling) {
        
        int answer = 0;
        for(String babblingWord : babbling){
            if(babblingWord.contains("yeye")){
                continue;
            }
            if(babblingWord.contains("woowoo")){
                continue;
            }
            if(babblingWord.contains("mama")){
                continue;
            }
            if(babblingWord.contains("ayaaya")){
                continue;
            }
            
            String replacedWord = babblingWord.replaceAll("(woo|ma|aya|ye)","");

            if(replacedWord.length() == 0){
                answer++;
            }
        }
        
        return answer;
    }
}
```
따라서 replaceAll을 활용하여 regexp를 활용해 woo