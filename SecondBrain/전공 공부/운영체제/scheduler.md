![[Pasted image 20250104225808.png|350]]
CPU에서 실행되기를 원하는 (ready 상태로 가기를 원하는) 스레드들이 ready queue에 저장된다.
scheduler는 이 중 CPU에서 실행될 프로세스를 선택하는 역할을 한다.

### dispatcher
스케쥴러가 선택한 프로세스를 실제로 cpu에서 실행할 수 있도록 하는 역할을 한다. 
ex) 
- context switching
- 커널 모드 -> 유저 모드 변경

