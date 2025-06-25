## 시간 복잡도
Sorted Set과 관련된 동작들은 대체로 O(log N)의 시간 복잡도를 가진다.
# Skip List

### 구현
![[Pasted image 20250625141246.png]]
구현은 Linked List와 거의 동일하지만, level이 특정 확률로 1씩 올라가면서 구현이 된다. (여기서는 1/4의 확률이며 최대 level은 32로 고정해두었다)

![[Pasted image 20250625141106.png]]
일정 확률에 의해 올라간 level은 Tree와 유사하게 탐색되며 logN의 탐색 시간을 갖는다.