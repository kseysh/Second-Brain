브랜치를 두 수의 대소를 비교해서 다음에 수행해야 하는 instruction의 주소를 바꿔버린다.

# Jumping
## jX instructions
![[Pasted image 20231005173442.png]]
ex)
```
cmpg a, b
setge c
```
=>
```
c = (b≥a)
```

ex)
```
cmpg a, b
jge label
```
=>
```
if(b≥a)
goto label
```

![[Pasted image 20231005173848.png]]
option을 주의깊게 볼 필요 x
-fno-if-conversion => if 관련 최적화를 
