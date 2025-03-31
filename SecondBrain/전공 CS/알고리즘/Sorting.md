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
		E[vacant] = E[vacant-1];
		xLoc = shiftVacRec(E, vacant-1, x);
	return xLoc;
}
```
### 특징
키를 비교하여 정렬하고 각 비교 후 최대 하나의 반전을 제거하는 모든 알고리즘은 최악의 경우 최소 n(n-1)/2 비교를 수행해야 하며 평균(n 요소에 대해) 최소 n(n-1)/4 비교를 수행해야 합니다.

삽입 정렬은 인접한 요소만 교환하여 locally하게 작동하는 알고리즘에 최적이다.
### 평균수행시간 분석
![[Pasted image 20250327214449.png|200]]
1. 정렬된 길이 i인 부분에서 원소가 삽입될 위치는 균등한 확률로 결정됨
2. 삽입될 위치가 1번부터 i+1번째 위치 중 하나일 확률이 동일하다고 가정하면, 각 위치에 삽입될 확률은 1 / (i + 1) 이다.
3. 위치 j에 삽입되면 비교 횟수는 j번 발생한다.
4. 따라서 평균 비교 횟수는 ![[Pasted image 20250327222501.png|100]]과 같다
5. ![[Pasted image 20250327222539.png|100]]이고, 따라서 ![[Pasted image 20250327222558.png|100]]이다.
6. 뒤의 i/i+1은 물어보자
### Worst-Case 분석
1. Index 1부터 시작하여 현재까지 정렬된 부분에서 적절한 위치를 찾아 삽입한다.
2. i 번째 원소를 올바른 위치에 삽입하기 위해 최대 i-1번의 비교 및 이동이 필요하므로 총 이동 횟수는 아래와 같다.
![[Pasted image 20250327214432.png|200]]

## Quick Sort
https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html
Quick Sort는 분할정복 방식을 기반으로 한 randomized sorting algorithm이다.
분할 단계에서 배열을 두 개의 서브 배열로 나누고, 이들 각각에 대해 재귀적으로 정렬을 수행하는 방식
### 의사코드
![[Pasted image 20250331153238.png|200]]
- Divide: pivot이라고 불리는 랜덤한 요소 x를 뽑고, L,E,G로 파티션을 만들고 나눈다.
	- L: x보다 작은 요소
	- E: x와 같은 요소
	- G: x보다 큰 요소
- Conquer: L과 G를 sort
- Combine: L,E와 G를 합친다.
각 삽입과 제거는 시퀀스의 시작 또는 끝에 있어서 O(1) 시간이 걸린다.
따라서, Quick Sort의 분할 단계는 O(n) 시간이 걸린다.
### Worst-Case 분석
Worst-Case: pivot이 고유한 최소 또는 최대 요소일 때 발생
L과 G 중 하나는 크기가 n − 1이고 다른 하나는 크기가 0이다.
실행 시간은 n + (n − 1) + ... + 2 + 1의 합계에 비례. 
따라서 Quick Sort의 최악의 실행 시간은 O(n<sup>2</sup>)
### 평균수행시간 분석
O(nlogn)
### 공간 복잡도 분석
### 예제
![[Pasted image 20250331152419.png|350]]

## In-Place Quick-Sort
배열의 원소들을 같은 배열 내에서 정렬하는 방식 
추가적인 메모리 공간을 거의 사용하지 않고, 분할 단계에서 배열의 원소들만 교환하여 정렬 수행
이름은 In-Place지만, 상수 공간이 아니다
![[Pasted image 20250328220218.png|300]]
#### example
![[Pasted image 20250328220729.png|400]]
j는 왼쪽에서 오른쪽으로 이동하며 pivot보다 크거나 같은 값을 찾음,
k는 오른쪽에서 왼쪽으로 이동하며 pivot보다 작거나 같은 값을 찾음
찾은 두 값의 위치를 교환함
![[Pasted image 20250328220841.png|300]]
이 과정을 j와 k가 교차할 때까지 반복한다.
## Merge Sort
![[Pasted image 20250331151358.png|200]]
C의 첫 번째 항목을 결정하십시오: A와 B의 첫 번째 항목 사이의 최소값입니다. 그것이 A의 첫 번째 항목이라고 가정해 봅시다. 그러면, C의 나머지 부분은 A의 나머지 부분을 B와 병합한 결과여야 한다.
### 의사코드
![[Pasted image 20250331151445.png|200]]
### Worst-Case 분석

### 평균수행시간 분석

### 공간 복잡도 분석