
![[Pasted image 20231106094253.png]]

reset은 commit history도 남지 않고, 직접 WorkingDir 및 Staging Area를 변경하기 때문에 특별한 경우 협업에서 충돌이 많아진다.
따라서 revert를 사용하는 경우에는 되돌린 history를 남겨 그 이유를 남길 수 있으며, 협업 상황에서 충돌을 최소화 할 수 있다.

실제 협업에서는 바로 전 commit을 revert하는 경우는 별로 없고, 주로 과거에 수정한 내용이나 merge한 내용을 풀어낼 때 revert하고 그 이유를 남기는 경우가 많다.

![[Pasted image 20231106095140.png]]
revert로 돌리는 경우는 이런 변경이 여기저기에 있고 복잡할 때 사용하면 유용하다.


## revert로 merge 풀기
revert를 하고 싶은 곳이 merge 커밋이라면 `git revert HEAD^`명령어만을 통해 revert를 진행할 수 없다.  
![[Pasted image 20231106175547.png]]
그래서 이 경우는 -m 즉, main 라인을 어떤 것으로 할 것인지도 동시에 넣어주어야 한다. `ex) git revert HEAD^ -m 1`
