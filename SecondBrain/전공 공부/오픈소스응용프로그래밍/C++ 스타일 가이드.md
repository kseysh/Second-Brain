




# define 가드
헤더 파일이 여러 번 포함되지 않기 위함
ex) foo/src/bar/baz.h 파일이 있을 때 foo 가드
![[Pasted image 20231011144709.png]]
# 인라인 함수
함수가 10라인 이하이면 인라인으로 하라
# include의 이름과 순서
자신의 헤더파일이 제일 먼저, 
C 라이브러리, 
C++ 라이브러리, 
다른 라이브러리들의 .h,
현재 프로젝트의 .h 순으로 나열한다.
모든 헤더 파일은 .이나 ..을 사용하지 않고 src의 하위요소로 나열되어야 한다.
ex ) google-awesome-project/src/base/logging.h 는 이와 같이 \#include 되어야 한다.
`#include "base/logging.h"`
각각의 부분 안에서 include는 알파벳 순서로 나열되어야 한다.
![[Pasted image 20231011145750.png]]
# 지역 변수
함수의 변수는 가능한 한 좁은 범위에 두고, 선언에서 초기화하라.
최대한 첫 번째 사용처에 가깝게 선언하라.
![[Pasted image 20231011145850.png]]
![[Pasted image 20231011145900.png]]

# 정적 변수와 전역 변수
클래스 타입의 정적 변수와 전역 변수를 금지한다.

# 생성자
생성자에서 복잡한 초기화 작업을 하는 것을 피하라.
생성자에서의 초기화는 initializer list를 활용
