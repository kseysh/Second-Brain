```java
class Solution {
    public int[] solution(int n, int m) {
        // 최대 공약수
        int a = Math.max(n, m);
        int b = Math.min(n, m);
        while (b != 0) {
            int r = a % b;
            a = b;
            b = r;
        }
 
        // 최대 공배수 = 두 수의 곱 / 최대공약수
        return new int[] { a, n * m / a };
    }
}
```

# 풀이법
- 최대공약수 gcd(a, b) 구하기    => **유클리드 호제법**을 이용

> 임의의 두 자연수 a, b(a > b)가 주어지고  
> a를 b로 나눈 나머지를 r(r = a % b)로 이라고 하자.  
>   
> a = b가되고 b = r이 되는데, 이때 **b가 0이 될 때의 a가 최대공약수**이다.

- 최소 공배수

> **최소 공배수 * 최대 공약수 = a * b** 임을 이용  
> 따라서 **최소 공배수 = a * b / 최대 공약수**