## example
![[Pasted image 20250203175816.png|400]]
OLD => update되기 전의 tuple
NEW => insert나 update 후의 tuple
# Trigger
DB에서 어떤 이벤트가 발생했을 때 자동적으로 실행되는 프로시저

### Trigger 정의 팁
row 단위가 아니라 statement 단위로 trigger가 실행될 수 있도록 할 수 있다. (mysql은 불가능)
update, insert, delete 등을 한 번에 감지하도록 설정 가능하다 (mysql은 불가능)

