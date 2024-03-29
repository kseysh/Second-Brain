# 제어의 역전 (IOC - Inversion of Control) 

이해가 쉽게 설명한 내용이 있어 인용해보려고 한다.
> 직장에 차를 몰고 가는 것은 내가 차를 제어하는 것이다. 직접 차를 운전하는 대신 운전 기사를 고용한다면 이것을 "제어의 역전"이라고 한다. 차를 직접 운전할 필요가 없이 운전자가 운전하게 함으로써 본업에 집중할 수 있다.

![[Pasted image 20240214150129.png|400]]
> 여기서 매개체 = IOC 컨테이너 라고 한다.
> 이 IOC 컨테이너는 개발자에게서 일임받은 제어권을 사용하여 의존성을 관리하고 인스턴스를 생성하여 주입해주고 메모리를 해제해주는 역할까지 해준다.

IOC란 클래스 간의 결합을 느슨하게 하여 테스트와 유지관리를 더 쉽도록 하는 설계 원칙이다. 
또한 메인 프로그램에서 컨테이너나 프레임워크로 객체와 객체의 의존성에 대한 제어를 옮기는 것을 말한다.

기존에는 객체의 의존관계를 개발자가 직접 처리하여 객체 간의 의존성이 강했지만 스프링에서는 **객체 생성과 의존 관계를 자바 코드로 직접 처리하는 것이 아니라 컨테이너가 대신 처리**하기 때문에 소스에 의존관계가 명시되지 않아 **낮은 결합도**를 유지해 **유지보수가 용이**해집니다.