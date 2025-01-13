## 기존 스레드 vs 가상 스레드
![[Pasted image 20250113231122.png|400]]

![[Pasted image 20250113231050.png|399]]

서버가 더 많은 요청 처리량과 컨텍스트 스위칭 비용을 줄이기 위해 나타난 스레드 모델

## 가상 스레드의 장점
![[Pasted image 20250113231211.png|250]]
Thread는 기본적으로 최대 2MB의 스택 메모리 사이즈를 가지기 때문에, 컨텍스트 스위칭 시 메모리 이동량이 크지만, 가상 스레드는 JVM에 의해 생성되기 때문에 커널 영역의 호출이 적고 메모리 크기와 컨텍스트 스위칭 비용도 적다.

## 가상 스레드의 구조
Platform Thread의 기본 스케쥴러는 ForkJoinPool을 사용하는데, 스케쥴러는 platform thread pool을 관리하고 Virtual Thread의 작업 분배 역할을 한다.

![[Pasted image 20250113231456.png]]
디버거를 통해 런타임의 Virtual Thread를 살펴보면
- VirtualThread는 `carrierThread`를 가지고 있습니다. 실제로 작업을 수행시키는 platform thread를 의미합니다. `carrierThread`는 `workQueue`를 가지고 있습니다.
- VirtualThread는 `scheduler`라는 ForkJoinPool을 가지고 있습니다. carrier thread의 pool 역할을 하고, 가상 스레드의 작업 스케줄링을 담당합니다.
- VirtualThread는 `runContinuation` 이라는 Virtual Thread의 실제 작업 내용(Runnable)을 가지고 있습니다.

## 가상 스레드의 동작 원리
![[Pasted image 20250113231744.png|500]]
1. 실행될 virtual thread의 작업인 runContinuation을 carrier thread의 workQueue에 push 합니다.
2. Work queue에 있는 runContinuation들은 forkJoinPool에 의해 work stealing 방식으로 carrier thread에 의해 처리됩니다.
3. 처리되던 runContinuation들은 I/O, Sleep으로 인한 interrupt나 작업 완료 시, work queue에서 pop되어 park과정에 의해 다시 힙 메모리로 되돌아갑니다.
## 가상 스레드의 단점
CPU bound 작업에서는 컨텍스트 스위칭에 대한 이점이 작용하지 않고, 어차피 경량 스레드도 플랫폼 스레드 위에서 동작하기 때문이다.
또한 가상 스레드 생성 및 스케쥴링 비용까지 포함되어 성능 낭비가 발생한다.

