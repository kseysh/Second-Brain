```java
class Solution {
    public int solution(String s) {
        char c = s.charAt(0);
        if(c == '-'){
            return 0 - Integer.parseInt(s.substring(1, s.length()));
        }
        if(c == '+'){
            return Integer.parseInt(s.substring(1, s.length()));
        }
        return Integer.parseInt(s);
    }
}
```