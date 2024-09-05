https://school.programmers.co.kr/learn/courses/30/lessons/12917#

```java
import java.util.Arrays;

class Solution {
    public String solution(String s) {
        char[] charArr = s.toCharArray();
        Arrays.sort(charArr);
        StringBuilder tmp = new StringBuilder(new String(charArr));
        return tmp.reverse().toString();
    }
}
```

1. Arrays는 import를 진행해야 한다.
2. char 배열을 reverseOrder하기 위해서는 StringBuilder를 사용한다.