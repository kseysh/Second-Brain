## Array Doubling
한 요소를 복사하는데 드는 비용: t
- (n+1)번째 요소 삽입이 배열 확장을 유발할 경우
	- 이 배열 확장 작업의 비용은 t * n
	- 이전 배열 확장들의 비용은 t * n/2 + t * n/4 + t * n/8 + ...로, 이는 t * n보다 작음
- 총 비용은 2t * n을 초과하지 않음


## Binary Search Tree
이진 탐색 트리를 inorder traversal하면 키들이 정렬된 리스트로 출력된다.
![[Pasted image 20250410155125.png|300]]
dot: 빈 tree

![[Pasted image 20250410155036.png|300]]
