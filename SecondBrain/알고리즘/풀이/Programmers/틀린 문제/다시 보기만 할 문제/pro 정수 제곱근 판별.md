```java
class Solution {
    public long solution(long n) {
        double sqrtValue = Math.sqrt(n);
        
        if(sqrtValue%1 == 0.0){ // 어떤 양의 정수 x의 제곱임
            long s = Double.valueOf(sqrtValue).longValue();
            return (s + 1) * (s + 1);
            
        }else{ // 어떤 양의 정수 x의 제곱이 아님
            final long notSquareValue = -1;
            return notSquareValue;
        }
    }
}
```

double이 정수인지 확인하기 위해서는 1로 나눠 나머지가 0.0인지 확인한다.
double to long은 아래와 같이 한다.
`Double.valueOf(sqrtValue).longValue();`
`(long) sqrtValue`

