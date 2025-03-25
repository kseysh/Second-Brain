# CPU [[Scheduler]]
- CPU scheduling 결정을 해야하는 상황
- 선점 스케쥴링
	- running state -> waiting state
	- running state -> terminate
- 비선점 스케쥴링
	- running state -> ready state
	- waiting state -> ready state

- policy(scheduling policy)와 mechanism(dispatcher)의 분리
![[Pasted image 20250325172314.png|300]]
# Dispatcher
context switching을 해주는 존재
