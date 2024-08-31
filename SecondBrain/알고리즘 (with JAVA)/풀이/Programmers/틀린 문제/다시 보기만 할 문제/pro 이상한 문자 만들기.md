```java
import java.util.*;
class Solution {
    public String solution(String s) {
        StringBuilder sb = new StringBuilder();
        int currentCount = 0;
        int strLength = s.length();
        boolean isEven = true;
        while(currentCount != strLength){
            if (s.charAt(currentCount) == ' '){
                isEven = true;
                sb.append(' ');
                currentCount++;
            }
            else if(isEven){
                sb.append(Character.toUpperCase(s.charAt(currentCount++)));            
                isEven = false;
            }else{
                sb.append(Character.toLowerCase(s.charAt(currentCount++)));
                isEven = true;
            }
        }
        
        return sb.toString();
    }
}
```

# 틀린 이유 
음... 뒤에 공백을 다 trim 해버렸기 때문...
Character.toUpperCase와
str.toUpperCase()를 잘 알아두자