```java
class Solution {
    public int solution(String t, String p) { // t : 0 ~ tLength - 1, p : 0 ~ pLength - 1
        int pLength = p.length();
        int firstPValue = p.charAt(0) - '0'; // 
        int tLength = t.length();
        long pLong = Long.parseLong(p);
        int answer = 0;
        for(int i = 0; i < tLength; i++){
            int firstTValue = t.charAt(i) - '0'; // 찾는 값의 첫 번째 수
            if(firstTValue <= firstPValue){
                int endIdx = i + pLength;
                if(endIdx <= tLength){
                    long subtractedT = Long.parseLong(t.substring(i, endIdx)); 
                    // subString에서 어차피 endIdx를 포함하지 않으므로 이렇게 들어와도 된다.
                    if(subtractedT <= pLong){
                        answer++;
                    }
                }
            }
        }
        return answer;
    }
}
```
## 배운점
String to Int는 항상 숫자의 범위가 비교 가능한지 확인해요.
만약, Long의 범위(약 10<sup>19</sup>)보다 큰 값이라면, str1.compareTo(str2) <= 0을 활용해요.