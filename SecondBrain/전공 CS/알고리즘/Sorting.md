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
