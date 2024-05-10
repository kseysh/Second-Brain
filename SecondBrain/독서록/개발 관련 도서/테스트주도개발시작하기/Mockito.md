Mockito는 일치하는 스텁 설정이 없을 경우 리턴 타입의 기본 값을 리턴한다.

### ArgumentMatchers 클래스에서 주의해야할 점
스텁을 설정할 메서드의 인자가 두 개 이상인 경우의 주의할 점
![[Pasted image 20240510212008.png]]

`mockList.set()` 메서드의 스텁을 설정할 때 첫 번째 인자는 `anyInt()` 메서드를 이용해서 임의의 int값에 일치하도록 했고 두 번째 인자는 "123"값을 사용해서 정확한 값에 일치하도록 했다. 
그리고 `mockList.set(5,"123")` 코드를 실행하고 있다. 

이 코드는 문제없이 동작할 것 같지만 실제로`mockList.set(5,"123")`코드를 실행하면 익셉션이 발생한다.
`ArgumentMatchers`의 `anyInt()`나 `any()`등의 메서드는 내부적으로 인자의 일치 여부를 판단하기 위해`ArgumentMatcher`를 등록한다. 

`Mockito`는 한 인자라도 `ArgumentMatcher`를 사용해서 설정한 경우 모든 인자를 `ArgumentMatcher`를 이용해서 설정하도록 하고 있다.
임의의 값과 일치하는 인자와 정확하게 일치하는 인자를 함께 사용하고 싶다면 `ArgumentMatchers.eq()` 메서드를 사용한다.

## 행위 검증
모의 객체의 역할 중 하나는 실제로 모의 객체가 불렸는지 검증하는 것이다.
![[Pasted image 20240510212443.png]]
then()은 메서드 호출 여부를 검증할 모의 객체를 전달받는다.
should() 메서드는 모의 객체의 메서드가 불려야 한다는 것을 설정하고 should() 메서드 다음에 실제로 불려야 할 메서드를 지정한다.
이 코드는 generate() 메서드가 GameLevel.EASY 인자를 사용해서 호출되었는지 검증한다.

정확한 값이 아니라 메서드가 불렸는지 여부가 중요하다면 ArgumentMatchers를 이용한다.
![[Pasted image 20240510212636.png]]

