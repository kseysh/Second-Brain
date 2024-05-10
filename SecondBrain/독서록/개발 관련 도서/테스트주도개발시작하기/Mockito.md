Mockito는 일치하는 스텁 설정이 없을 경우 리턴 타입의 기본 값을 리턴한다.

### ArgumentMatchers 클래스에서 주의해야할 점
스텁을 설정할 메서드의 인자가 두 개 이상인 경우의 주의할 점
![[Pasted image 20240510212008.png]]

`mockList.set()` 메서드의 스텁을 설정할 때 첫 번째인자는anyInt()메서드를이용해서임의의 i nt값에일치하도록했고두번째인자는1 23"값을사용해서정확한값에일치하도록했다. 그리고mockList. ser(5," 123") 코드를실행하고있다. 이코드는문제없이동작할것같지만 실제로mockList.set(5,1 23")코드를실행하면익셉션이발생한다.
Ar gu m e n t Mat c h er s 의 a n yI nt ( ) 나 a n y ( ) 등의 메 서드는 내 부적으로 인자의 일치 여 부를 판 단하기위해Argument Mat cher를등록한다. Mocki to는한인자라도ArgumentMatcher를
사 용 해 서 설 정 한 경 우 모 든 인 자 를 Ar g u m e n t M a t c h e r 를 이 용 해 서 설 정 하 도 록 하 고 있 다 .
임의의값과일치하는인자와 정확하게일치하는인자를함께 사용하고싶다면[리스트C. 6]과 같 이 A r g u m e n t M a t c h e r s . c g ( ) 메 서 드 를 사 용 해 야 한 다.