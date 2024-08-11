```java
class Solution {
    public String solution(String phone_number) {
        int phoneNumberLength = phone_number.length();
        String showingNumber = phone_number.substring(phoneNumberLength - 4, phoneNumberLength);
        
        StringBuilder sb = new StringBuilder();
        
        for(int i = 0 ; i < phoneNumberLength - 4; i++){
            sb.append('*');
        }
        sb.append(showingNumber);
        
        return sb.toString();
    }
}
```
https://school.programmers.co.kr/learn/courses/30/lessons/12948#