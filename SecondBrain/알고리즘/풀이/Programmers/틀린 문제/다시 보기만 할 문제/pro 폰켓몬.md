https://school.programmers.co.kr/learn/courses/30/lessons/1845
```java
class Solution {
    public int solution(int[] nums) {
        boolean[] poketmonTypeChecker = new boolean[200001];
        int getCapacity = nums.length/2;
        int typeCount = 0;
    
        for(int num: nums){
            if(!poketmonTypeChecker[num]){
                typeCount++;
                poketmonTypeChecker[num] = true;
            }
        }
    
        return Math.min(typeCount, getCapacity);
    }
}
```

## 다른 사람의 풀이
```java
import java.util.HashSet;

class Solution {
    public int solution(int[] nums) {

            HashSet<Integer> hs = new HashSet<>();

            for(int i =0; i<nums.length;i++) {
                hs.add(nums[i]);
            }

            if(hs.size()>nums.length/2)
                return nums.length/2;

            return hs.size();
    }
}
```
HashSet을 이용하여 중복된 요소를 제거하였다.
이 방식이 더 맞는 방식인듯