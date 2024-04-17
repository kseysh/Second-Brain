# BDDMockito와 ArgumentCaptor
```java
ArgumentCaptor<Long> ac = ArgumentCaptor.forClass(Long.class);
// 모의 객체를 메서드를 호출할 때 전달한 객체를 담는 기능을 제공한다.

BDDMockito
	.then(userRepository) // userRepository 모의 객체의
	.should() // 특정 메서드가 호출되었는지 검증할 것이다.
	.findUserById(ac.capture());  // findUserById에서 어떤 값이 사용되었는지를 확인할 것이다.
								// 이는 anyLong()으로 사용하여 그냥 호출되었는지를 확인할 수도 있다.

System.out.println("ac.getValue() = " + ac.getValue()); // 모의 객체 호출시 담긴 값이 나온다.
// ArgumentCaptor에서 나온 값을 이용해 assertEqual을 해도 좋다!
```

