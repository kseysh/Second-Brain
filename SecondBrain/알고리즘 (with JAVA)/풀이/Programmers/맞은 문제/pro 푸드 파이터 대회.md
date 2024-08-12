# 나의 풀이
```java
class Solution {
    int[] foods;
    StringBuilder sb = new StringBuilder();
    int currentIdx = 1;
    public String solution(int[] food) {
        foods = food;
        createString();
        return sb.toString();
    }
    
    public void createString(){
        int foodCapacity = foods[currentIdx];
        final int foodCalory = currentIdx;
        if(foodCapacity % 2 == 1){
            foodCapacity--;
        }
        
        for(int i = 0; i < foodCapacity / 2; i++){
            sb.append(foodCalory);
        }
        ++currentIdx;
        if(foods.length == currentIdx){
            sb.append(0);
        }else{
            createString();   
        }
        for(int i = 0; i < foodCapacity / 2; i++){
            sb.append(foodCalory);
        }
    }
}
```

# 다른 사람의 풀이
```java
class Solution {
    public String solution(int[] food) {
        String answer = "0";

        for (int i = food.length - 1; i > 0; i--) {
            for (int j = 0; j < food[i] / 2; j++) {
                answer = i + answer + i; 
            }
        }

        return answer;
    }
}
```
저는 StringBuilder를 가지고 풀었는데 생각해보니 이렇게 푸는 방식도 맞겠네요.