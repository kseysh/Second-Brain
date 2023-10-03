# Big-O 
f(x)가 g(x)에 대한 Big-O Notation이면,
즉, f(x) = O(g(x))이면
f(x) ∈ O(g(x))이고, 
![[Pasted image 20231003203723.png]]
이다.

## Big-O 증명
Big-O를 증명하려면, f(x)에서 x가 k 이상일 때, C의 배수로 표현할 수 있으면, Big-O를 증명할 수 있다.


f(x) = x<sup>2</sup> + 2x + 1이 O(x<sup>2</sup>)임을 증명

x>1보다 클 때, x<x<sup>2</sup> 이고, 1<x<sup>2</sup>이다.

따라서 ![[Pasted image 20231003204446.png]]
이고 이는 C=4이고, k = 1인 Big-O Notation이라고 할 수 있다.
