# Git이란?
오픈소스 분산 버전 관리 시스템의 일종
## Git과 GitHub
![[Pasted image 20230921013114.png]]

## 기초 리눅스 명령어
~는 홈 디렉터리를 나타냄

--명령어--
`pwd` : 현재 작업 중인 디렉터리명 출력
`ls` : 현재 디렉터리 내 파일 리스트 출력
-l (long) 옵션은 상세 정보 출력, -a (all) 옵션은 숨겨진 파일 및 디렉터리까지 출력
`mkdir <디렉터리명>` : 디렉터리 생성
`rm <파일/디렉터리명>` : 파일/디렉터리 삭제 (디렉터리의 경우 -r (recursive) 옵션으로 삭제 가능)
`vim <파일명>` : VIM 에디터로 새로운 파일 생성
`cat <파일명>` : 파일내용 출력
`git init` : .git 디렉터리 생성
vi 명령어 : 
`:w` : 입력 시 저장
`:q` : 에디터 종료 (!를 뒤에 붙이면 강제로 종료함)


## Git에서의 버전 관리
git에서 저장소를 관리하기 위해 사용하는 세 가지 공간

- working tree
	- 소스코드를 직접 수정 및 저장하는 디렉터리
- Stage
	- 버전관리를 수행할 대상이 되는 파일들이 모인 공간
- Repository
	- 저장소라고 하며 각 버전이 저장되어 있는 공간

![[Pasted image 20230921014546.png]]

## Git 명령어
`git status` : 저장소 상태 확인
`git log` : 버전 이력 확인 가능
- 가장 위쪽에 commit hash가 나옴
- HEAD -> master라는 표시는 최신 버전임을 의미함
- commit을 누가 언제 했는지와 commit 메시지가 출력됨
- `git log --graph --all` 명령으로 분기된 지점을 포함하여 전체 이력 출력
`git diff` : 최신 버전과 현재 working tree 내의 차이점들을 요약하여 확인 가능
`git reset HEAD <파일명>` : stage에 추가된 파일을 다시 stage에서 제거하고 싶은 경우 `git restore --staged <파일명>`도 동일한 명령어
`git checkout--<파일명>` : working tree에 수정한 파일을 저장소에 있던 버전으로 다시 덮어쓰는 명령
- 만약 수정된 파일이 stage에 포함되어 있었다면 stage에서도 제거되고 working tree에서의 파일을 원본으로 되돌리는 것

`git reset HEAD^` : commit된 파일의 내용을 이전 버전으로 되돌리고 싶은 경우
`git reset --hard <commit hash>` : commit된 파일의 내용을 특정 버전으로 되돌리고 싶은 경우
`git reflog` : reset을 통해 커밋 내용을 되돌릴 때 확인 및 복구하는 명령어
`git branch <브랜치이름>` : 새로운 브랜치 생성시의 명령어
`git branch` : 모든 브랜치 리스트 및 현재 브랜치 출력
`git merge <브랜치 이름>` : 현재 선택된 브랜치의 HEAD로 merge 명령 뒤에 입력한 브랜치가 병합됨
- main에 issue 브랜치를 병합하기 위해서는 main 브랜치로 현재 브랜치 전환 후 merge를 수행하여야 함
`git checkout <브랜치 명>`  : 특정 브랜치로 이동
`git branch -d <브랜치 이름>` :  브랜치 삭제
`git remote add origin <원격 저장소 주소>` : 원격 저장소 연결
- origin이란 기본값으로 지정된 원격 저장소 명
`git remote` : 현재 추가된 원격 저장소 확인
- -v 옵션을 통해 더 자세한 정보 확인 가능
`git push <원격 저장소> <로컬 브랜치>` : 로컬 브랜치의 내용을 원격으로 연결되어 있는 저장소에 백업 및 반영 가능
- -u 옵션을 이용하면 처음에만 원격 저장소명 및 로컬 브랜치 명을 써주면, 이후에는 `git push` 명령만으로 간편하게 동일 대상에 대해 push 수행 가능
`git pull <원격저장소> <원격 저장소의 브랜치>` : 원격 저장소의 브랜치 내용을 로컬 저장소로 가지고 오는 명령어

## stage 공간의 필요성
수정된 부분 중 일부분만 commit 가능하다.
특정 파일만 골라서 commit 가능하다.
commit 전 코드 리뷰 및 테스트 용



