# 나의 풀이
```java
class Solution {
    String word;
    int wordLength;
    int currentWordIdx;
    int firstWordIdx;
    int answer;
    
    public int solution(String s) {
        word = s;
        wordLength = word.length();
        currentWordIdx = 0;
        firstWordIdx = 0;
        answer = 0;
        
        while(currentWordIdx <= wordLength - 1){
            checkOneWord();
        }
        
        return answer;
    }
    
    public void checkOneWord(){
        char firstWord = word.charAt(currentWordIdx);  

        currentWordIdx++;
        int firstWordCount = 1;
        int otherWordCount = 0;
        if(currentWordIdx >= wordLength){
            answer++;
            return;
        }
        
        
        while(firstWordCount != otherWordCount){
            if(currentWordIdx == wordLength){ // 이 부분을 정의해주지 않아 "cc"가 값으로 들어왔을 때 IndexOutOfRange 에러가 발생하였다. 항상 index 관련해서는 경계를 체크하자.
                answer++;
                return;
            }
            
            char currentWord =  word.charAt(currentWordIdx);
            currentWordIdx++;

            if(firstWord == currentWord){
                firstWordCount++;
            }else{
                otherWordCount++;
            }
            
            
            if(firstWordCount == otherWordCount){
                answer++;
                return;
            }
        }
    }
}
```
# 틀렸던 점
```java
if(currentWordIdx == wordLength){ 
// 이 부분을 정의해주지 않아 "cc"가 값으로 들어왔을 때 
// IndexOutOfRange 에러가 발생하였다. 항상 index 관련해서는 경계를 체크하자.
	answer++;
	return;
}
```