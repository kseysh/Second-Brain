### 불변성
Stream은 내부적으로 불변 데이터 흐름을 유지
### 병렬 처리
for문: 직접 Thread나 ForkJoin을 활용해야함
Stream: .parallelStream()으로 간단히 병렬 처리 가능
### 성능
소규모 데이터: for > Stream
대규모 데이터: for < Stream

Stream은 내부 객체 생성과 람다 호출 오버헤드로 인해 소규모 데이터에서 불리하지만, parallelStream으로 인해 멀티코어를 활용할 수 있어 대규모 데이터에서 유리함