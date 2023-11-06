커밋을 취소하고 싶을 때 사용하는 명령어.

## Option

![[Pasted image 20231106092644.png]]
### Case 1) 커밋을 작성한 시점으로 되돌리기
c2 이후에 작성한 모든 내용이 잘못되었거나 필요없는 경우 c2라는 커밋을 작성한 시간으로 되돌리기 위해 사용

`git reset --hard c2` => HEAD^도 가능하다.

### Case 2) C3 commit 작성 바로 전으로 돌아가기
![[Pasted image 20231106092902.png]]

c3의 커밋바로 이전으로 돌아가 작성된 내용을 날리지 않기 위해 사용

`git reset --mixed `
=> mixed는 defau