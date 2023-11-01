#### rebase and merge

> rebase  
> => 브랜치의 시작점을 다른 commit으로 옮겨주는 행위  
> => rebase를 이용해서 신규브랜치의 시작점을 main 브랜치 최근 commit으로 옮긴 다음 fast-forward merge 해서 commit log를 더 깔끔하게 하기 위해

=> 새로운 브랜치 이동-> git rebase main -> 브랜치가 main 브랜치 끝으로 이동하므로 fast-forward merge하면 된다.  
장점 : 커밋 로그를 깔끔하게 관리할 수 있다.  
[![img1 daumcdn](https://user-images.githubusercontent.com/69035864/279557519-2632d7a0-0383-4b54-9874-066977c9a3ca.png)](https://user-images.githubusercontent.com/69035864/279557519-2632d7a0-0383-4b54-9874-066977c9a3ca.png)