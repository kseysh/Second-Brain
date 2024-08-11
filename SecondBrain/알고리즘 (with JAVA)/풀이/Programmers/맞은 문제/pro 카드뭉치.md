```java
class Solution {
    public String solution(String[] cards1, String[] cards2, String[] goal) {
        int currentCard1Index = 0;
        int currentCard2Index = 0;
        
        int goalLength = goal.length;
        int cards1Length = cards1.length;
        int cards2Length = cards2.length;
        for(int i = 0; i < goalLength; i++){
            if(currentCard1Index < cards1Length){ // index는 cards1의 범위 안에 있어야 함
                if(cards1[currentCard1Index].equals(goal[i])){ // cards1에 찾는게 있음
                    currentCard1Index++;
                    continue;
                }
            }
            if(currentCard2Index < cards2Length){
                if(cards2[currentCard2Index].equals(goal[i])){// cards2에 찾는게 있음
                    currentCard2Index++;
                    continue;
                }
            }
            return "No"; // 조건에 맞아 continue 되지 않으면 NO 출력
        }
        return "Yes";// 중간에 끊기지 않고 잘 왔다면 YES 출력
    }
}
```