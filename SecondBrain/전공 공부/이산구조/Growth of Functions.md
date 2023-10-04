# Big-O 
f(x)가 g(x)에 대한 Big-O Notation이면,
즉, f(x) = O(g(x))이면
f(x) ∈ O(g(x))이고, 
![[Pasted image 20231003203723.png]]
이다.

## Big-O 증명
Big-O를 증명하려면, f(x)에서 x가 k 이상일 때, C의 배수로 표현할 수 있으면, Big-O를 증명할 수 있다.
![[Pasted image 20231003204915.png]]

ex)
f(x) = x<sup>2</sup> + 2x + 1이 O(x<sup>2</sup>)임을 증명

x>1보다 클 때, x<x<sup>2</sup> 이고, 1<x<sup>2</sup>이다. => k = 1

따라서 ![[Pasted image 20231003204446.png]] => C = 4
이고 이는 C=4이고, k = 1인 Big-O Notation이라고 할 수 있다.

![[Pasted image 20231003205036.png]]

ex2)
7x<sup>2</sup> 은 O(x<sup>3</sup>)이다.
C=7, k=1

ex3)
n<sup>2</sup>은 O(n)이 아니다.
n<sup>2</sup> ≤ Cn 이면, n ≤ C이고,
이는 n > k에서 모두 만족할 수 없기 때문에 성립하지 않는다.

> Big-O를 찾는 문제만 나오고 증명문제는 나오지 않음!


## 특이한 Big-O
n!에 대한 Big-O Notation => O(n<sup>2</sup>)
![[Pasted image 20231003210332.png]]
log n! 에 대한 Big-O Notation => n! ≤ n<sup>n</sup> 이고, log(n!) ≤ n\*log(n)
그러므로, log(n!)은 O(n\*log(n))이다.

## 함수의 성장 속도
![[Pasted image 20231003210508.png]]
크기 비교시 사용하므로 알아두기

# Big-Omega
알고리즘을 실행시켰을 때, 최소로 걸릴 수 있는 시간
x>k일 때, |f(x)| ≥C|g(x)| 인 함수

f(n) = O(g(n)) 참2. f(n) = Ω(g(n)) 참 이 두 가지가 참일 경우에만  
f(n) = θ(g(n))  을 사용할 수 있다.


### 증명
f(x) = 8x<sup>3</sup> +5x<sup>2</sup> + 7은 g(x) = x<sup>3</sup> 에서 Ω(g(x))임을 증명하라

모든 x에서 f(x) = 8x<sup>3</sup> +5x<sup>2</sup> + 7 ≥ 8x<sup>3</sup> 이므로 
f(x) = 8x<sup>3</sup> +5x<sup>2</sup> + 7은 g(x) = x<sup>3</sup> 에서 Ω(g(x))이다.

# Big-Theta Notation
Big-O와 Big-Omega가 같을 때 (증가율이 같을 때), 이를 Big-Theta라고 한다.

### 증명
![[Pasted image 20231004105814.png]]임을 증명하라
![[Pasted image 20231004105839.png]]
![[Pasted image 20231004105904.png]]

# Time Complexity
### Linear Search의 Worst case
![[Pasted image 20231004112817.png]]
θ(n)
### Linear Search의 Average case
![[Pasted image 20231004112911.png]]
θ(n)
### Binary Search의 Worst case
이진 탐색의 시간 복잡도는 O(logN)으로 배열을 전수 조사하는 O(N)에 비하면 상대적으로 빠른 탐색 알고리즘에 속한다. O(logN)만에 값을 찾을 수 있는 이유는 중간을 기준으로 탐색 대상을 절반씩 줄여나가기 때문이다.

이진 탐색은 내가 찾고자 하는 값이 정렬된 배열의 중간 값보다 크면 중간값을 포함한 하위 값들은 탐색 대상에서 제외된다. 반대로 찾고자 하는 값이 배열의 중간 값보다 작으면 중간 값을 포함한 상위 값들은 탐색에서 제외된다.

![[Pasted image 20231004112928.png]]
θ(log n)
### Bubble Sort의 Worst case
버블 정렬은 버블이 수면 위를 올라오는 듯 옆에 있는 데이터와 비교하여 더 작은 값을 앞으로 보내는 정렬입니다.
![[img 1.gif]]
![[Pasted image 20231004113455.png]]
θ(n<sup>2</sup>), Worst, Average, Best 동일
느리고 효율성 떨어
### Insertion Sort의 Worst Case
삽입 정렬은 데이터를 순서대로 뽑아서 적절한 위치를 찾아 삽입함으로써 완성하는 정렬
![[img.gif]]
![[Pasted image 20231004114000.png]]
θ(n<sup>2</sup>), Worst, Average는 동일하고
이미 정렬되어 있는 Best의 경우  O(n)
### Matrix Multiplication
![[Pasted image 20231004114156.png]]
두개의 n n matrix가 있다면
product에 n<sup>2</sup>의 entries가 있을 것이고 그 entries는 각각 n개의 곱셈, n-1개의 덧셈이 필요할 것이다 따라서 O(n<sup>3</sup>)

### Boolean Product

