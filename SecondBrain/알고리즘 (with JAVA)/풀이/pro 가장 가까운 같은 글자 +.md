```java
class Solution {
    public int[] solution(String s) {
        int sLength = s.length();
        int[] answer = new int[sLength];
        int[] alphabetCount = new int[26];
        
        for(int i =0; i< 26; i ++){
            alphabetCount[i] = -1;
        }
        
        for(int currentIndex = 0; currentIndex < sLength; currentIndex++){
            int alphabetIndex = s.charAt(currentIndex) - 'a';
            int previousIndex = alphabetCount[alphabetIndex] + 1;
            
            if(previousIndex == 0){
                answer[currentIndex] = -1;
            } else {
                answer[currentIndex] = currentIndex - previousIndex + 1;
            }
            alphabetCount[alphabetIndex] = currentIndex;
        }
        
        return answer;
    }
}
```

## 틀린 점
기본 초기화 값인 0을 사용하게 되면, Index값을 저장할 때 0이 저장된 것인지 기본 초기화값이어서 0이 된건지 구분하지 못하게 된다. 이에 유의하자.

## 다른 사람의 풀이
```java
import java.util.*;

class Solution {
    public int[] solution(String s) {
        int[] answer = new int[s.length()];
        HashMap<Character,Integer> map = new HashMap<>();
        for(int i=0; i<s.length();i++){
            char ch = s.charAt(i);
            answer[i] = i-map.getOrDefault(ch,i+1);
            map.put(ch,i);
        }
        return answer;
    }
}
```
HashMap을 활용하였다. getOrDefault를 잘 활용하자.