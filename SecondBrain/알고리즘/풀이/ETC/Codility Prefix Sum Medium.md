# 문제
A non-empty array A consisting of N integers is given. A pair of integers (P, Q), such that 0 ≤ P < Q < N, is called a _slice_ of array A (notice that the slice contains at least two elements). The _average_ of a slice (P, Q) is the sum of A[P] + A[P + 1] + ... + A[Q] divided by the length of the slice. To be precise, the average equals (A[P] + A[P + 1] + ... + A[Q]) / (Q − P + 1).

For example, array A such that:

    A[0] = 4
    A[1] = 2
    A[2] = 2
    A[3] = 5
    A[4] = 1
    A[5] = 5
    A[6] = 8

content_copy

contains the following example slices:

> - slice (1, 2), whose average is (2 + 2) / 2 = 2;
> - slice (3, 4), whose average is (5 + 1) / 2 = 3;
> - slice (1, 4), whose average is (2 + 2 + 5 + 1) / 4 = 2.5.

The goal is to find the starting position of a slice whose average is minimal.

Write a function:

> `class Solution { public int solution(int[] A); }  content_copy    `

that, given a non-empty array A consisting of N integers, returns the starting position of the slice with the minimal average. If there is more than one slice with a minimal average, you should return the smallest starting position of such a slice.

For example, given array A such that:

    A[0] = 4
    A[1] = 2
    A[2] = 2
    A[3] = 5
    A[4] = 1
    A[5] = 5
    A[6] = 8

content_copy

the function should return 1, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [2..100,000];
> - each element of array A is an integer within the range [−10,000..10,000].

## 풀이

```java
class Solution {
    public int solution(int[] A) {
        int N = A.length;
        
        // 초기값 설정: 첫 번째 2개짜리 슬라이스를 기준으로 잡음
        double minAvg = (A[0] + A[1]) / 2.0;
        int minIdx = 0;

        // 배열 전체 순회 (마지막 요소 전까지만 보면 됨)
        for (int i = 0; i < N - 1; i++) {
            
            // 1. 길이 2짜리 슬라이스 확인 (P, P+1)
            double avg2 = (A[i] + A[i+1]) / 2.0;
            if (avg2 < minAvg) {
                minAvg = avg2;
                minIdx = i;
            }

            // 2. 길이 3짜리 슬라이스 확인 (P, P+1, P+2)
            // 마지막 인덱스에서는 길이 3을 만들 수 없으므로 범위 체크 (i < N - 2)
            if (i < N - 2) {
                double avg3 = (A[i] + A[i+1] + A[i+2]) / 3.0;
                if (avg3 < minAvg) {
                    minAvg = avg3;
                    minIdx = i;
                }
            }
        }

        return minIdx;
    }
}
```
생각으로는 최솟값은 2개 또는 3개에서 나오는 건가? 라고 생각했지만, 수학적으로 증명하지 못해 결국 풀지 못했다.

### 수학적으로 2개 또는 3개에서 나오는 이유 
길이가 4인 슬라이스 [a, b, c, d]의 평균은 앞부분 [a, b]의 평균과 뒷부분 [c, d]의 평균의 평균과 같습니다. 만약 앞부분 평균이 더 작다면? 굳이 길이 4를 택할 필요 없이 앞부분(길이 2)을 선택하면 됩니다. 반대로 뒷부분이 더 작다면? 뒷부분을 선택하면 됩니다. 즉, 길이가 4 이상인 모든 짝수 슬라이스는 길이가 2인 더 작은 평균을 포함하고 있습니다.

## Double을 사용하는 이유
컴퓨터는 소수를 완벽하게 저장하지 못하고 근사값으로 저장합니다(부동소수점 방식).

float (32bit): 유효 자릿수가 약 7자리입니다.
double (64bit): 유효 자릿수가 약 15~16자리입니다.
오답이 될 위험이 float가 더 크다.
또한, Java에서는 소수 리터럴을 기본적으로 double로 취급하여 float는 항상 형 변환을 해야한다.