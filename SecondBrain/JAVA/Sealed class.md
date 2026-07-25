내가 관리하는 특정 클래스에게만 상속하고 싶을 때 사용

상속 계층 내의 엄격한 제어를 통해 프로그램의 구조를 명확하게 유지하기 위해 사용

ex)

```java
sealed class SealedStudyGroup permits Algo, CS, Java {
	
}

```

![[Pasted image 20260725120757.png]]