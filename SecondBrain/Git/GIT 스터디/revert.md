
![[Pasted image 20231106094253.png]]

reset은 commit history도 남지 않고, 직접 WorkingDir 및 Staging Area를 변경하기 때문에 특별한 경우 협업에서 충돌이 많아진다.
따라서 revert를 사용하는 경우에는 되돌린 history를 남겨 그 이유를 남길 수 있으며, 협업 상황에서 충돌을 최소화 할 수 있다.

실제 협업에서는 바로 전 commit을 revert하는 경우는 별로 없고, 주로 과거에 수정한 내용이나 merge한 내용을 풀어낼 때 revert하고 그 이유를 남기는 경우가 많다.



