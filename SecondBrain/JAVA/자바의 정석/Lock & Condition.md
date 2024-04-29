# Lock과 Condition을 이용한 동기화
![[Pasted image 20240429104519.png]]
## 낙관적인 잠금(Optimistic Lock)
일단 무조건 저지르고 나중에 확인
![[Pasted image 20240429104458.png]]
## ReentrantLock을 이용한 동기화
![[Pasted image 20240429104548.png]]
## synchronized대신 lock()과 unlock()을 사용
![[Pasted image 20240429104640.png]]
## ReentrantLock과 Condition으로 스레드를 구분해서 wait() & notify()
![[Pasted image 20240429104726.png]]
## ReentrantLock과 Condition의 생성방법
![[Pasted image 20240429104752.png]]
# volatile - cache와 메모리간의 불일치 해소
성능 향상을 위해 변수의 값을 core의 cache에 저장해놓고 작업
여러 스레드가 공유하는 변수에는 volatile을 붙여야 항상 메모리에서 읽어온다.
![[Pasted image 20240429104910.png]]