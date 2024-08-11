```java
class Solution {
    public String solution(String s, String skip, int index) {
        boolean[] skipped = new boolean[26];
        
        StringBuilder sb = new StringBuilder();
        
        for(char c : skip.toCharArray()){
            skipped[c - 'a'] = true;
        } // skipped 추가
        
        for(int i = 0; i < s.length(); i++){
            int count = 1; // skipped를 포함해 뒤로 간 수
            int nextCount = 0; // 5개 뒤여야 함
            int charNum = s.charAt(i) - 'a';
            int checkNum = -1;
            while(nextCount < index){
                checkNum = charNum + count;
                while(checkNum >= 26){
                    checkNum -= 26;
                }
            
                if(!skipped[checkNum]){ // skip되지 않는다면
                    nextCount++;
                }    
                count++;
            }
            sb.append((char)(checkNum + 'a'));
        }
        
        return sb.toString();
    }
}
```

## 틀린 부분
checkNum에서 26을 빼도 26보다 클 수도 있다!

```java
while(checkNum >= 26){
	checkNum -= 26;
}
```

```java
if(checkNum > 26){
	checkNum -= 26;
}
```

## 다른 사람의 풀이
```java
class Solution {
    public String solution(String s, String skip, int index) {
        StringBuilder answer = new StringBuilder();

        for (char letter : s.toCharArray()) {
            char temp = letter;
            int idx = 0;
            while (idx < index) {
                temp = temp == 'z' ? 'a' : (char) (temp + 1);
                if (!skip.contains(String.valueOf(temp))) {
                    idx += 1;
                }
            }
            answer.append(temp);
        }

        return answer.toString();
    }
}
```