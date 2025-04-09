## Insertion Sort
![[Pasted image 20250327214346.png|400]]
### 의사 코드
```c
void insertionSort(Element[] E, int n){
	int xindex;
	for (xindex = 1; xindex < n; xindex++)
		Element current = E[xindex];
		key x = current.key;
		int xLoc = shiftVacRec(E, xindex, x);
		E[xLoc] = current;
	return;
}
```
```c
int shiftVacRec(Element[] E, int vacant, Key x){
	int xLoc;
	if (vacant == 0)
		xLoc = vacant;
	else if (E[vacant-1].key ≤ x)
		xLoc = vacant;
	else
		E[vacant] = E[vacant-1]; // 한 칸 뒤로 보내서 vacant를 만들어 줌
		xLoc = shiftVacRec(E, vacant-1, x);
	return xLoc;
}
```
### 특징
키를 비교하여 정렬하고 각 비교 후 최대 하나의 반전을 제거하는 모든 알고리즘은 최악의 경우 최소 n(n-1)/2 비교를 수행해야 하며 평균(n 요소에 대해) 최소 n(n-1)/4 비교를 수행해야 합니다.

삽입 정렬은 인접한 요소만 교환하여 locally하게 작동하는 알고리즘에 최적이다.
### Worst-Case 분석
1. Index 1부터 시작하여 현재까지 정렬된 부분에서 적절한 위치를 찾아 삽입한다.
2. i 번째 원소를 올바른 위치에 삽입하기 위해 1 ~ i-1번의 비교 및 이동이 필요하므로 총 이동 횟수는 아래와 같다.
![[Pasted image 20250327214432.png|200]]
### 평균수행시간 분석
![[Pasted image 20250327214449.png|200]]
1. 정렬된 길이 i인 부분에서 원소가 삽입될 위치는 균등한 확률로 결정됨
2. 삽입될 위치가 1번부터 i+1번째 위치 중 하나일 확률이 동일하다고 가정하면, 각 위치에 삽입될 확률은 1 / (i + 1) 이다.
3. 위치 j에 삽입되면 비교 횟수는 j번 발생한다.
4. 따라서 평균 비교 횟수는 ![[Pasted image 20250327222501.png|100]]과 같다
5. ![[Pasted image 20250327222539.png|100]]이고, 따라서 ![[Pasted image 20250327222558.png|100]]이다.
6. 뒤의 i/i+1은 물어보자


## Divide and Conquer
![[Pasted image 20250331172122.png|300]]
## Quick Sort
https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html
Quick Sort는 분할정복 방식을 기반으로 한 randomized sorting algorithm이다.
분할 단계에서 배열을 두 개의 서브 배열로 나누고, 이들 각각에 대해 재귀적으로 정렬을 수행하는 방식
### 의사코드

![[Pasted image 20250409153302.png|300]]
p: pivot 위치
입력 시퀀스 S를 pivot x를 기준으로 세 부분으로 나눔
L: x보다 작은 원소
E: x와 같은 원소
G: x보다 큰 원소
#### partition의 시간복잡도
원소를 하나씩 꺼내고 삽입하는 연산이 모두 리스트 앞이나 뒤에서 발생
→ 즉, 각 연산은 O(1) 시간
• 전체 원소 n개에 대해 각각 한 번씩만 다루므로: O(n)
### Worst-Case 분석
Worst-Case: pivot이 고유한 최소 또는 최대 요소일 때 발생
L과 G 중 하나는 크기가 n − 1이고 다른 하나는 크기가 0이다.
실행 시간은 n + (n − 1) + ... + 2 + 1의 합계에 비례. 
따라서 Quick Sort의 최악의 실행 시간은 O(n<sup>2</sup>)

## In-Place Quick-Sort
배열의 원소들을 같은 배열 내에서 정렬하는 방식 
추가적인 메모리 공간을 거의 사용하지 않고, 분할 단계에서 배열의 원소들만 교환하여 정렬 수행
![[Pasted image 20250328220218.png|300]]
S: 정렬할 배열
l, r: 현재 정렬하려는 구간의 시작과 끝 idx
h: 피벗보다 작은 원소들의 끝 idx
k: 피벗과 같은 원소들의 끝 idx
피벗보다 작은 구간과 큰 구간에 대해 다시 퀵 정렬을 수행
#### example
![[Pasted image 20250328220729.png|400]]
j는 왼쪽에서 오른쪽으로 이동하며 pivot보다 크거나 같은 값을 찾음,
k는 오른쪽에서 왼쪽으로 이동하며 pivot보다 작거나 같은 값을 찾음
찾은 두 값의 위치를 교환함
![[Pasted image 20250328220841.png|300]]
이 과정을 j와 k가 교차할 때까지 반복한다.
### 예제
![[Pasted image 20250331152419.png|350]]
## Merge Sort
두 개의 시퀀스 A와 B가 오름차순으로 정렬되어 있을 때, 이 둘을 병합하여 하나의 정렬된 시퀀스 C를 만든다.
![[Pasted image 20250331151358.png|200]]

### 의사코드
![[Pasted image 20250409155536.png|200]]
W(n) = n-1
![[Pasted image 20250331151445.png|200]]

### Worst-Case 분석
![[Pasted image 20250409160033.png|300]]
이고, 마스터 정리에 따라
![[Pasted image 20250409160049.png|100]]
### 예시
![[Pasted image 20250409160433.png|400]]
## Heap and Heapsort
### Heap Structure

### Partial order tree property
