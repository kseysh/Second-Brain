## union
![[Pasted image 20251008223546.png|300]]
uf 배열을 만들고 -1로 초기화한다.


union 1 5
union 2 3
union 1 3 (1의 root는 2이기 때문에 2와 1을 연결해주어야 한다. 항상 루트와 루트를 연결함)
![[Pasted image 20251008223907.png|300]]

## Find
![[Pasted image 20251008224008.png|300]]
find 3 -> root 1까지 가서 찾음 
root인지 아닌지는 -1이 적혀있는지 아닌지를 확인하면 된다.