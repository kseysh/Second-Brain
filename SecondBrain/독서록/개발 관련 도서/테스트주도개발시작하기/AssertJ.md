JUnit은 테스트 실행을 위한 프레임워크를 제공하지만 단언에 대한 표현력이 부족하다.

# AssertJ 기본 사용법
```java
assertThat(actual).검증메서드(expected)
```
### 검증 메서드 종류
![[Pasted image 20240510213307.png]]

#### Comparable 인터페이스를 구현한 타입이나 int, double과 같은 숫자 타입의 검증 메서드
![[Pasted image 20240510213355.png]]
![[Pasted image 20240510213402.png]]
#### boolean, Boolean 타입을 위한 검증 메서드
![[Pasted image 20240510213425.png]]

#### String에 대한 추가 검증 메서드
![[Pasted image 20240510213441.png]]
![[Pasted image 20240510213449.png]]

![[Pasted image 20240510213458.png]]

![[Pasted image 20240510213507.png]]

#### 숫자에 대한 추가 검증 메서드
![[Pasted image 20240510213525.png]]

#### 날짜/시간에 대한 검증 메서드
![[Pasted image 20240510213542.png]]

![[Pasted image 20240510213553.png]]

#### 컬렉션에 대한 검증 메서드
![[Pasted image 20240510213607.png]]

![[Pasted image 20240510213614.png]]
![[Pasted image 20240510213619.png]]

#### Exception 관련 검증 메서드
![[Pasted image 20240510213643.png]]
![[Pasted image 20240510213657.png]]

## as()와 describedAs()로 설명 달기
![[Pasted image 20240510213806.png]]
