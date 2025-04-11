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
### 평균수행시간 분석 (시)
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
이진 트리 T가 힙 구조를 가지려면, 다음 조건을 모두 만족해야 한다
- T는 깊이 h-1까지는 완전 이진 트리여야 한다
- 모든 리프 노드는 깊이 h 또는 h-1에 있어야 한다
- 깊이 h에 있는 모든 leaf node 경로는 깊이 h - 1에 있는 leaf node 경로보다 왼쪽에 있어야 한다.
### Partial order tree property
트리의 어떤 노드든지 그 자식 노드들보다 값이 크거나 같아야 한다.
#### example
![[Pasted image 20250409162758.png|300]]
위 둘 heap 아님, 아래 둘 heap
## Heapsort Strategy
![[Pasted image 20250410153144.png|300]]
정렬할 요소들이 힙(heap) 구조로 정렬되어 있다면,
루트에서 요소를 하나씩 제거하고
남은 요소들을 다시 정렬하여 부분 순서 트리 특성을 유지시키는 과정을 반복함으로써,
역순으로 정렬된 시퀀스를 만들 수 있습니다.
### 의사 코드
![[Pasted image 20250409163104.png|300]]
E: 정렬할 요소들의 집합
n: 요소의 수
최대 힙을 이용하여 큰 값부터 차례로 배열 뒤에 저장함으로써 정렬 수행
![[Pasted image 20250409163116.png|300]]
힙에서 최대값을 삭제하는 절차
H의 가장 마지막(오른쪽 아래)의 요소를 변수 K에 복사
해당 요소를 힙에서 삭제
힙 삭제 후 fixHeap호출로 힙 성질 복원
![[Pasted image 20250409163129.png|300]]
힙 성질을 복구하는 함수, 루트 노드로 올라온 노드 K를 자기 자리로 찾게 하기
•	만약 H가 리프 노드라면:
	→ K를 현재 위치에 삽입
•	그렇지 않으면:
	•	왼쪽/오른쪽 서브힙 중 루트 키가 더 큰 쪽을 선택
	•	만약 K.key ≥ 큰 서브힙의 루트 키:
		→ K를 현재 루트에 삽입
	•	아니면:
		→ 큰 서브힙의 루트 키를 현재 루트로 복사
		→ fixHeap(선택된 서브힙, K) 재귀 호출

fixHeap은 최대 2h번의 키 비교가 필요함
전체 HeapSort의 시간 복잡도는 2log(n)이므로, 힙 정렬은 O(n log n) 시간 복잡도를 가짐
## Construct Heap
Input H: 힙 구조이지만, partial order tree property을 반드시 만족하지는 않는 상태 (즉, 최소 힙, 최대 힙의 성질을 보장하지 않는 구조)
Output H: 동일한 노드들로 구성된 H를 partial order tree property 속성을 만족하도록 재배열한 힙으로 변환
![[Pasted image 20250410150557.png|300]]

![[Pasted image 20250411143218.png|200]]
위 구조를 이용하여 leaf를 포함한 subtree까지 내려가고, 가장 작은 subtree부터 하나씩 heap 조건을 만족시키면 됨
## Array를 이용한 Heap 구현
1부터 n까지의 array
left child: 2i
rigth child: 2i + 1
parent: ![[Pasted image 20250410150720.png|50]]
## Worst Case 분석
fixHeap 함수가 힙에서 수행하는 비교 연산 수는 최대 2 * log(k)
k는 현재 힙의 노드 개수이며, 로그는 힙에 높이에 비례하는 성질 반영
최악의 경우, 루트에서 leaf까지 내려가며 자식과 비교 및 교환을 하기 때문
n개의 원소를 힙 정렬에서 delete할 때, 총 비교 연산 수
![[Pasted image 20250410151222.png|200]]
•	힙 정렬에서 최악의 경우 수행되는 키 비교 횟수는 2n log(n) + O(n)입니다.
•	2n log(n)은 주된 비교 횟수(삭제 시마다 최대 2 log(k)씩), O(n)은 초기 힙 구성 시의 비교 횟수
## 가속 힙 정렬
일반적인 fixHeap 연산은 최악의 경우 2h번의 비교가 필요함

1.	빈 위치(vacant position)를 트리의 절반 깊이(h/2)까지 필터링합니다.
2.	K가 그 위치의 부모보다 큰지 확인합니다.
•	Yes (크다): 빈 위치를 거품처럼 위로 올려서 K가 들어갈 위치로 이동시킵니다.
•	No (작다): 빈 위치를 다시 절반 깊이만큼 아래로 재귀적으로 필터링합니다!

이 방식은 힙에서 요소를 아래로 내려보내는 대신, 좀 더 효율적으로 올릴 수 있는지를 판단해 비교 횟수를 줄이는 기술입니다.
결과적으로 비교 연산 횟수를 줄여 성능을 약 2배 향상시킬 수 있습니다.

![[Pasted image 20250410152443.png|300]]
K: 삽입 대상이 되는 값
루트를 시작으로 아래로 이동하면서 vacant가 아래로 내려감
vacStop 위치에 도달하면 적절한 위치에 삽입됨
### 수도 코드
![[Pasted image 20250410151648.png|300]]
![[Pasted image 20250410151854.png|300]]
![[Pasted image 20250410151907.png|300]]
### fixHeapFast 시간 복잡도 분석
vacant가 bubbleUpHeap 또는 Promote의 작용으로 인해 한 레벨씩 이동할 때마다 한 번의 비교가 발생하고, 이 비교의 총 횟수는 힙의 높이인 h이다.
bubbleUpHeap이 호출되지 않고, fixHeapFast가 기저 조건에 도달한다고 가정해 봅시다. 
이 경우, 방향을 반대로 바꿔야 하는지를 확인하기 위해 log(h) 번의 체크가 필요합니다.
따라서 최악의 경우, fixHeapFast는 h + log(h) 번의 비교를 수행한다.
### Accelerated Heapsort 시간 복잡도 분석
k개의 노드를 가진 힙에서 fixHeapFast가 수행하는 비교 횟수는 최대 log(k) 
모든 삭제 연산을 포함한 총 비교 횟수: ![[Pasted image 20250410153623.png|100]]
Accelerated Heapsort가 최악의 경우 수행하는 키 비교 횟수: ![[Pasted image 20250410153754.png|100]]
## Radix Sort
비교를 하지 않고 정렬하는 방법
기수 = 자릿수
![[Pasted image 20250410222955.png|500]]
Unsorted file -> First Pass: 48081은 bucket 1에 97342는 bucket 2에 ...
First Pass -> Second Pass: 48081은 bucket 8에 48001은 bucket 0에 ...

![[Pasted image 20250410230745.png|500]]
remL(remain List): Linked List 구조로 각 노드가 다음 숫자를 가리킴

### 수도 코드
![[Pasted image 20250410230942.png|400]]
![[Pasted image 20250410230955.png|400]]
![[Pasted image 20250410231005.png|400]]
## 시간 복잡도 분석
- distribute 단계: Θ(n) 시간 소요
	- 각 숫자를 해당 자리수에 따라 버킷에 분배하는 작업
	- 전체 n개의 원소를 한 번씩 확인하며 분배하므로 Θ(n) 시간 복잡도를 가짐
- Combine 단계도 Θ(n) 시간 소요
	- 버킷에 분배된 숫자들을 다시 하나로 합치는 과정도 모든 원소를 한 번씩 지나야 하므로 Θ(n)입니다.
- 자리 수 개수가 일정하면,
	- ex) 모든 숫자가 5자리면 총 Pass 횟수는 5회로 고정
	- 이 때 전체 수행 시간은 각 Pass마다  Θ(n)이므로, 총 시간은 Θ(n) × 고정된 Pass 수 = Θ(n)
즉, Radix sort는 자릿수 고정시 선형 시간 정렬이 될 수 있음

## 공간 복잡도 분석
버킷 분배와 재조합을 위해 Linked List와 같은 보조 구조가 필요함
이 때 필요한 추가 공간도 원소 개수 n에 비례함
