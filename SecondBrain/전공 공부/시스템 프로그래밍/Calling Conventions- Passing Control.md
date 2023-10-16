Passing Control이 필요한 이유 : jump는 다시 돌아올 수 없지만 다시 돌아와야 하므로

![[Pasted image 20231016212347.png]]
큰 흐름 : call을 이용해 jump를 하고, return을 이용해 call의 다음줄로 이동한다.

call은 `pushq pc` 와 `jmp lable`이 합쳐진 것

돌아올 때의 함수 : ret
스택의 top에서 pop을 해서 돌아오는 역할을 한다.