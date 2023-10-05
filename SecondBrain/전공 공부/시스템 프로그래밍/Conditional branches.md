브랜치란 두 수의 대소를 비교해서 다음에 수행해야 하는 instruction의 주소를 바꿔버린다.
if같은 곳에서 사용
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

## Old Style Conditional Branch Example
![[Pasted image 20231005173848.png]]
option을 주의깊게 볼 필요 x
-fno-if-conversion => if 관련 최적화를 하지 마라
.L4 => assembly어에서 사용하는 label => 라인의 주소를 담아둠
return value가 %rax이므로 movq로 rax를 담아두고 subq로 뺀 값을 넣어준다.

컴파일러가 코드를 바로 변환하지는 못해서 if문을 goto를 이용해 바꿔주는 작업이 필요하다.
![[Pasted image 20231005174206.png]]
![[Pasted image 20231005175428.png]]
하지만 jump instruction은 비용이 비싼 instruction이다

# Conditional Moves
