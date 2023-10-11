




# define 가드
헤더 파일이 여러 번 포함되지 않기 위함
ex) foo/src/bar/baz.h 파일이 있을 때 foo 가드
![[Pasted image 20231011144709.png]]
# 인라인 함수
함수가 10라인 이하이면 인라인으로 하라
# include의 이름과 순서
C 라이브러리, C++ 라이브러리, 다른 라이브러리들의 .h, 현재 프로젝트의 .h 순으로 나열한다.
모든 헤더 파일은 .이나 ..을 사용하지 않고 src의 하위요소로 나열되어야 한다.
ex ) google-awesome-project/src/base/logging.h 는 이와 같이 \#include 되어야 한다.
`#incldu`