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
### 평균수행시간 분석
![[Pasted image 20250327214449.png|200]]
### 공간 복잡도 분석

### Worst-Case 분석
![[Pasted image 20250327214432.png|200]]


## 알고리즘
### 의사코드
### 평균수행시간 분석
### 공간 복잡도 분석

### 시간 복잡도 분석
