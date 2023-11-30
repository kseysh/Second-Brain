초기 프로젝트 파일을 복제하여 local로 가져올 때 파일을 만들고 git init 및 origin으로 원격 저장소 등록 후 저장소 내용을 받아온다.

![[Pasted image 20231115123854.png]]

두 명령어는 같은 기능을 한다!

#### 기본 명령어
`git clone "원격 저장소 주소" <지정할 폴더>`
#### 특정 Branch or Tag만 Clone
`git clone --branch [branch or Tag] "원격 저장소 주소"`
#### 최신 변경만 반영된 소스코드 Clone
`git clone --depth=[원하는 깊이] "원격 저장소 주소"`
