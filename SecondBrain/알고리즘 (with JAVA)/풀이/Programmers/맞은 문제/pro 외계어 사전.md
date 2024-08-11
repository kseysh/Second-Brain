```java
import java.util.*;

class Solution {
    public int solution(String[] spell, String[] dic) {
        int spellLength = spell.length;
        Map<Character, Boolean> spellMap = new HashMap<>();
        
        for(int j = 0; j < spellLength; j++){
            spellMap.put(spell[j].charAt(0), false);            
        } // map 초기화
        
        for(int i = 0; i < dic.length; i++){ // 확인할 케이스
            String s = dic[i]; // 확인할 스트링
            
            for(int j = 0; j < s.length(); j++){
                char c = s.charAt(j);
                if(spellMap.containsKey(c)){ // spell에 존재하지 않으면 넘김
                    if (spellMap.get(c) == true) { // 이미 사용한 단어이면 넘김
                        break;
                    }
                    spellMap.replace(c, true);
                } 
            }
            
            boolean flag = true;
            for(int j = 0; j < spellLength; j++){
                if(!spellMap.get(spell[j].charAt(0))){
                    flag = false;
                    break;
                }
            }
            if(flag){
                return 1;
            }
            
            for(int j = 0; j < spellLength; j++){
                spellMap.replace(spell[j].charAt(0), false);
            } // map 초기화
        }
        
        
        return 2;
    }
}
```
 문제를 잘 읽자
 