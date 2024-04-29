# Collector란?
`collect()`는 `Collector`를 매개변수로 하는 [[스트림]]의 최종연산
`Collector`는 수집에 필요한 메서드를 정의해놓은 *인터페이스*이다.
`Collectors`클래스는 다양한 기능의 `Collector`를 **구현한 클래스**를 제공한다.

## `Collectors`의 메서드
### 스트림을 컬렉션으로 변환 - `toList()`, `toSet()`, `toMap()`, `toCollection()`
![[Pasted image 20240429151541.png]]
### 스트림의 통계정보 제공 - `counting()`, `summingInt()`, `maxBy()`, `minBy()`
![[Pasted image 20240429151548.png]]
### 스트림을 리듀싱 - `reducing()`
![[Pasted image 20240429151556.png]]
### 문자열 스트림의 요소를 모두 연결 - `joining()`
![[Pasted image 20240429151624.png]]
### 스트림의 요소를 2분할 - `partitioningBy()`
![[Pasted image 20240429151605.png]]
### 스트림의 요소를 그룹화 - `groupingBy()`
![[Pasted image 20240429151633.png]]

## Collector 구현
### Collector인터페이스를 구현하는 클래스를 작성
![[Pasted image 20240429151836.png]]
### Collector가 수행할 작업의 속성 정보를 제공 - `characteristics()`
![[Pasted image 20240429151843.png]]
### 문자열 스트림의 모든 요소를 연결하는 컬렉터 - `ConcatCollector`
![[Pasted image 20240429151851.png]]
