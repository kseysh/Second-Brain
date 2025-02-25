## example
![[Pasted image 20250203175816.png|400]]
OLD => update되기 전의 tuple
NEW => insert나 update 후의 tuple
# Trigger
DB에서 어떤 이벤트가 발생했을 때 자동적으로 실행되는 프로시저

### Trigger 정의 팁
row 단위가 아니라 statement 단위로 trigger가 실행될 수 있도록 할 수 있다. (mysql은 불가능)
update, insert, delete 등을 한 번에 감지하도록 설정 가능하다 (mysql은 불가능)
trigger를 발생시킬 디테일한 조건을 지정할 수 있다. (mysql은 불가능)
### Trigger 사용 시 주의사항
소스 코드로는 발견할 수 없는 로직이기 때문에 어떤 동작이 일어나는지 파악하기 어렵고 문제가 생겼을 때 대응하기 어렵다.
=> trigger는 가시적이지 않아서 개발도, 관리도, 문제 파악도 힘들어진다.
