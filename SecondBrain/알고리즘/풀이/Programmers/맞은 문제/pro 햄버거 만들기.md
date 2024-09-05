```java
import java.util.*;

class Solution {
    public int solution(int[] ingredient) {
        final int BREAD = 1;
        final int VEGETABLE = 2;
        final int MEET = 3;
        // 빵 - 야채 - 고기 - 빵 순서
        Stack<Integer> stk = new Stack<>();
        int answer = 0;
        
        for(int i = 0; i < ingredient.length; i++){
            int currentFood = ingredient[i];
            stk.push(currentFood);
            if(currentFood == BREAD){
                int stackSize = stk.size();
                if(stackSize >= 4){
                    if(stk.get(stackSize - 2) == MEET){
                        if(stk.get(stackSize - 3) == VEGETABLE){
                            if(stk.get(stackSize - 4) == BREAD){
                                stk.pop();
                                stk.pop();
                                stk.pop();
                                stk.pop();
                                answer++;
                            }    
                        }
                    }
                }
            }
        }
        return answer;
    }
}
```

# 다른 사람의 풀이
```java
class Solution {
    public int solution(int[] ingredient) {
        int[] stack = new int[ingredient.length];
        int sp = 0;
        int answer = 0;
        for (int i : ingredient) {
            stack[sp++] = i;
            if (sp >= 4 && stack[sp - 1] == 1 && stack[sp - 2] == 3 && stack[sp - 3] == 2 && stack[sp - 4] == 1) {
                sp -= 4;
                answer++;
            }
        }
        return answer;
    }
}
```
Stack 클래스를 사용하지 않고 stack의 원리만 차용할 수도 있습니다.
굳이 pop을 하는 것은 도움이 되지 않기 때문입니다.