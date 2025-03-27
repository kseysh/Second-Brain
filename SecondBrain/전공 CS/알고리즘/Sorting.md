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

삽입 정렬은 인접한 요소만 교환하여 "로컬" 작동하는 알고리즘에 최적입니다. 하지만, 그것은 최고의 정렬 알고리즘은 아니다.
### 평균수행시간 분석
![[Pasted image 20250327214449.png|200]]
1. Insertion Sort에서 i번째 원소를 삽입할 때, 평균적으로 삽입할 위치를 배열의 절반 위치에서 찾게 된다.
2. 따라서, i번째 원소가 삽입될 때 필요한 평균 비교 횟수는
### 공간 복잡도 분석

### Worst-Case 분석
1. Index 1부터 시작하여 현재까지 정렬된 부분에서 적절한 위치를 찾아 삽입한다.
2. i 번째 원소를 올바른 위치에 삽입하기 위해 최대 i-1번의 비교 및 이동이 필요하므로 총 이동 횟수는 아래와 같다.
![[Pasted image 20250327214432.png|200]]


## 알고리즘
### 의사코드
### 평균수행시간 분석
### 공간 복잡도 분석

### 시간 복잡도 분석
