# Search Algorithm?

# **Finding the Maximum Element in a Finite Sequence**
![[Pasted image 20231015212314.png]]

# **Linear Search**
![[Pasted image 20231015212423.png]]

# **Bubble sort**
![[Pasted image 20231003194849.png]]
둘 둘 씩 비교해서 sorting 기준에 의해 위치를 정확한 위치로 이동시켜준다.

## **수도 코드**
![[Pasted image 20231003195058.png]]
# **Insertion sort**
![[Pasted image 20231003195207.png]]
2는 3과 비교했을 때, 3보다 작으므로 맨 앞에 둔다.
4는 3과 비교했을 때, 3보다 크므로 그대로 있는다.
1은 4,3,2와 비교했을 때, 2보다 작으므로 맨 앞에 둔다.
계속 앞의 수열들을 sorting되게 한다.

## **수도 코드**
![[Pasted image 20231003200334.png]]
> while a<sub>j</sub> > a<sub>i</sub>
> i := i + 1 까지가 while절 이다.

# Greedy Algorithms
## Optimization problems
가능한 input에 대해서 최소나 최대 값을 구하는 문제
ex) 최단거리 탐색
코인이 25원짜리, 10원짜리, 5원짜리, 1원짜리가 있을 때,
67원을 최소의 코인으로 완성하려면 어떻게 해야 하는가?
## 수도코드
![[Pasted image 20231003201652.png]]
## Greedy의 한계
만약 코인이 25원짜리, 10원짜리, 1원짜리만 있다면,
31원이 있을 때, 25원 1개 1원짜리 6개로 7개의 코인을 사용해야 하지만
10원 3개, 1원 한 개로 코인을 만들면, 4개의 코인으로 31원을 만들 수 있다.
큰 그림을 보지 못하는 문제가 발생할 수 있다.







