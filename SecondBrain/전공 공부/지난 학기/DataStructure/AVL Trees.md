=> 'height-balance'를 만족하는 Binary Tree
>height-balance : T의 모든 노드 v가
>`h(v.left())-h(v.right()) =< 1` 를 만족하는 것

AVL Tree의 높이 : `O(log n)`
=> balanced-Binary-Tree이기 때문!

AVL Tree는 일반 Binary Tree처럼 insert를 진행한다. 하지만, 일반 Binary Tree와 다르게 insert를 진행한 후, height-balance를 만족하지 않는다면, 균형을 맞춰주는 작업(Restructure)을 진행한다.

#### AVL Tree Insertion
```
BST Insertion - O(logn)
Find the bottom-most "imbalanced" node z, on the insertion Path - O(logn)
Restructure at z - O(1)
```
single restructure => O(1)
maintaining heights is O(1)
#### AVL Tree deletion
```
BST Deletion - O(logn)
Find the bottom-most imbalanced node z, on the deletion path - O(logn)
Restructure at z - O(logn)
```
single restructure => O(1)
maintaining heights is O(log n)

> 왜 insertion과 다르게 deletion은 restructure의 time complexity가 O(logn)인가?
> 삭제 시 삭제한 곳에서 restructure가 발생한다. 그러면 height-balance가 맞지 않는 node에서의 h가 1 줄어들게 되고 그것으로 인해서 윗 레벨에서의 height-balance가 맞지 않게 될 수도 있다. 그렇게 되면 최대 트리의 높이만큼 restructure를 진행할 수 있고 따라서 deletion에서의 restructure의 time complexity는 O(log n)이다. 

>insertion의 경우에는 노드를 삽입하면서 h가 증가하여 height-balance가 맞지 않게 된 경우이므로 restructure로 인해 높이를 -1 시켜주면  +1,-1로 인해 height-balance가 맞게 된다.

### Restructure
Restucture를 진행할 때에는 first unbalanced node를 찾아 그 노드를 기준으로 rotation을 진행한다.

#### case1 : single rotation
![[sign.jpg]]
right-right => LR(Left Rotation) 진행
left-left => RR(Right Rotation) 진행

#### case2 : double rotation
![[ddddd.jpg]]
right-left => RR(Right Rotation) > LR(Left Rotation)
left-right => LR(Left Rotation) > RR(Right Rotation)

##### rotation을 두 번 수행하는 이유
![[rea.jpg|400]]
double rotation에서 첫 번째 rotation을 하는 이유는 a, b, c를 일자로 만들어주어 이후 두 번째 rotation에서 height를 줄일 수 있게 하기 위함이다.

#### Rebalancing after a Removal
![[removal.jpg|500]]
위의 상황을 right-tie 라고 부르는데, 이 상황은 insertion에서 right-right case의 restructure처럼 수행하면 된다. 
마찬가지로 left-tie 상황에서도 left-left case의 restructure를 수행하면 된다.

## AVL Tree Performance
find() => `O(log n) Time`
>tree의 높이가 `O(log n)` 이므로

put() => `O(log n) Time`
>넣을 자리를 탐색하는 cost : `O(log n)` 
> 높이를 유지하기 위한 restructure의 cost : `O(log n)`
> rotation은 상수 시간에 수행되며 최악의 경우 트리의 높이만큼 수행하므로

erase() => `O(log n) Time`
>삭제할 값을 탐색하는 cost : `O(log n)`
>높이를 유지하기 위한 restructure의 cost : `O(log n)`
> rotation은 상수 시간에 수행되며 최악의 경우 트리의 높이만큼 수행하므로



[[Binary Search Tree]]