```java
class Solution {
    public long solution(int price, int money, int count) {
        long neededMoney = -1;
        long countLong = count;
        if(count % 2 == 0){
            neededMoney = ((countLong + 1) * (countLong / 2)) * price;
        }else{
            neededMoney = ((countLong + 1) * (countLong / 2) + (countLong / 2 + 1)) * price;
        }
        
        long answer = neededMoney - money;

        return (answer > 0) ? answer : 0;
    }
}
```
틀린 이유 : int overflow가 일어났습니다. 
overflow 테스트시에는 직접 한 번 값을 계산해보는 것이 좋을 것 같습니다.
# 다른 사람의 풀이
```java
class Solution {
    public long solution(long price, long money, long count) {
        return Math.max(price * (count * (count + 1) / 2) - money, 0);
    }
}
```
파라미터를 바꿔도 되나봅니다..