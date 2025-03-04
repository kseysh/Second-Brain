# Heap 메모리
힙 영역에서 GC가 발생한다.
클래스 인스턴스, 배열 데이터가 쌓이는 곳
shared memory라고도 불리며 여러 스레드에서 공유하는 데이터들이 저장되는 메모리이다.
# GC의 역할
- 메모리 할당
- 사용 중인 메모리 인식
- 사용하지 않는 메모리 인식
# GC의 구조
GC는 각 영역에 할당된 크기의 메모리가 허용치를 넘을 때 수행한다.
필요할 때 알맞은 GC 방식으로 시스템에 적용하는 정도가 적당하다.
보통의 경우 Young 영역과 Old 영역의 비율은 1:2~1:9 정도이다.
![[Pasted image 20250304200358.png|300]]
### perm
거의 사용이 되지 않음
JDK 8부터 사라진 영역
### Young
Stop-the-World를 이용해 Young 영역 처리 (Collection 수행시 application 정지)
#### Eden
메모리에 객체 생성시 저장되는 곳
Eden 영역에 데이터가 꽉차면 GC 후 살아 남은 객체는 Survivor 영역 중 한 곳으로 이동한다.
Survivor로 들어가지 못할 정도로 큰 객체는 바로 Old 영역으로 이동한다.
#### Survivor
1,2로 나뉘며 한 곳은 항상 비어 있다.
할당된 Survivor 영역이 차면 GC가 되며 Eden 영역에 있는 객체와 꽉 찬 Survivor 영역에 있는 객체가 GC 진행 후 비어 있는 Survivor 영역으로 이동한다.
### Old
Mark-sweep-compact collection 알고리즘을 따른다.
# GC의 종류
### Minor GC
young 영역에서의 GC
### Major GC (Full GC)
Old, Perm 영역에서의 GC

# GC 방식

### Serial Collector
Young 영역과 Old 영역이 serial하게 처리되며 하나의 CPU를 사용
Collect 수행시 application 정지
=> 클라이언트 종류의 장비에서 많이 사용
=> 대기 시간이 많아도 문제되지 않는 시스템에서 사용
### Parallel Collector
Young 영역에서의 Collect를 parallel로 처리
CPU가 대기 상태로 남아 있는 것을 최소화하는 것이 목적이며 여러 CPU를 사용하는 서버에 적합

### Parallel Compacting Collector
=> 여러 CPU를 사용하는 서버에 적합
### Concurrent Mark-Seep Collector (CMS)
힙 메모리 영역의 크기가 클 때 적합
2개 이상의 프로세서를 사용하는 서버에 적당 => 웹 서버
### Garbage First Collector (G1)
