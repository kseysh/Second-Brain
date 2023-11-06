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
=> mixed는 default이므로 생략 가능하다.

## 정리
![[Pasted image 20231106093142.png]]
soft는 잘 사용하지 않는다.

## reset의 위험성
`git reset --soft` => reset 명령어 중 가장 안전한 명령어
`git reset --mixed` => Working Dir에서 한 일이 사라지진 않지만 Staging Area의 내용이 손실될 수 있다.
`git reset --hard` => 저장되지 않은 내용들이 모두 손실된다.
