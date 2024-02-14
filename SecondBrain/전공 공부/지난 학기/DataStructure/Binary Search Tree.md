inorder traversal시 증가하는 수 대로 방문을 시행한다.

BST 구현
**lookup**
```
lookup(k)
v<-root(T)
while(v is not null)
	if v.key() = k then
		return v
	else if v.key() > k then
		v<-v.left()
	else if v.key() < k then
		v<-v.right()
return v
```
> Q. Search 함수는 linear recursion인가?
> linear recusion이란?
> 자신을 호출할 때 항상 하나의 재귀 호출만 수행하는 재귀 함수 호출 방식
> Search 함수는 트리의 높이에 비례하는 재귀 구현이으로 linear recursion이 아니다.

Time complexity : `O(h) Time`
 최악의 경우 루트에서 리프까지 트리의 높이만큼만 순회하므로

**lookup'(k)**
```
lookup(k)
v<-root(T)
while(v is not null)
	if v.key() = k then
		return v
	else if v.key() > k then
		v<-v.left()
	else if v.key() < k then
		v<-v.right()
if v is null then
	return v.parent
else
	return v
```

	insertion을 위한 lookup

Time complexity : `O(h) Time`
  최악의 경우 루트에서 리프까지 트리의 높이만큼만 순회하므로

**insertion(k,v)**
```
u<-lookup'(k)
create the left child if u.key()>k
create the right child if u.key()<k
```
새로 만들어진 노드는 항상 leaf 노드에 들어간다. (Binary Search Tree의 특성)
Time complexity : `O(h) Time`
O(h)Time인 lookup함수를 사용하므로

**deletion(k)**
```
u<-lookup(k)
if u is null then
	return
else
	(case1) u is leaf node
		remove u from T
	(case 2) u is non-leaf node
		(case 2-1) u has only a single child w
			u를 지우고 u 자리에 w의 서브트리를 넣는다.
		(case 2-2) u has two children
			1. find v that follows u in an in-order traversal
				* v : in-order successor of u			
			2. delete u
			3. move v to u's node position
			4. move w to v's position
```
위의 수도코드에서 w는 v의 서브트리이다.
> in-order successor란?
>주어진 노드의 값보다 크면서 가장 작은 값을 가진 노드. 
>즉, 노드의 오른쪽 서브트리에서 가장 왼쪽에 위치한 노드 (혹은 노드의 왼쪽 서브트리에서 가장 오른쪽에 위치한 노드)
>따라서 in-order successor는 왼쪽 자식이 있을 수 없으며 자식이 0이거나 1이다.

Time complexity : `O(h) Time`
in-order successor를 찾기 위해 leaf까지 내려가는 부분과 lookup함수가 O(h) Time 이므로

**closest(k)**
특정 값에 가장 가까운 값을 찾는 함수
Time complexity : `O(h) Time`
lookup함수 처럼 같은 값을 찾다가 찾는 값보다 큰 값을 찾게 되면 그 전 값과 비교해서 둘 중에 더 가까운 값을 리턴

### BST Performance
BST는 O(n)의 space를 지닌다.

Time complexity에서
h는 
	Worst Case : O(n)
	Best Case : O(log n)을 지닌다.
 
[[Dictionary & Maps]]
[[AVL Trees]]