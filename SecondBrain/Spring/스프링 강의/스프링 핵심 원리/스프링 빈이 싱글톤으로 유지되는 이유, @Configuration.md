스프링 컨테이너는 싱글톤 레지스트리다. 따라서 스프링 빈이 싱글톤이 되도록 보장해주어야 한다. 
하지만, 스프링이 자바 코드까지 어떻게 하기는 어렵기 때문에 클래스의 바이트코드를 조작하는 라이브러리를 사용한다.

# 스프링 빈, 어떻게 싱글톤으로 유지되었을까?
@Bean을 호출해서 스프링 빈을 생성할 때, `memberRepository`가 아래와 같이 호출 될 때, 
1. 스프링 컨테이너가 스프링 빈에 등록하기 위해 `@Bean`이 붙어있는 `memberRepository()`호출
2. `memberService()`로직에서 `memberRepository()`호출
3. `orderService()`로직에서 `memberRepository()`호출
memberService, orderService, memberRepository를 모두 호출하게 되면 new MemberRepository가 세 번 호출될텐데, 검증해보면 memberRepository는 new를 이용해 한 번만 생성되게 된다. 어떻게 한 번만 생성되게 되었을까?


```java
@Test
 void configurationDeep() {

	ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

	// AppConfig도 스프링 빈으로 등록된다.
	AppConfig bean = ac.getBean(AppConfig.class);

    System.out.println("bean = " + bean.getClass());
	// 출력: bean = class hello.core.AppConfig$$EnhancerBySpringCGLIB$$bd479d70
```
`AppConfig` 스프링 빈을 조회해서 클래스 정보를 출력하면  `bean = class hello.core.AppConfig$$EnhancerBySpringCGLIB$$bd479d70` 로 나온다.
순수한 클래스라면 다음과 같이 출력되어야 한다. `class hello.core.AppConfig`

이것은 내가 만든 클래스가 아니라 스프링이 **CGLIB라는 바이트코드 조작 라이브러리를 사용**해서 **AppConfig 클래스를 상속받은 임의의 다 른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것**

이 임의의 `AppConfig@CGLIB`이라는 클래스가 바이트 코드를 조작해서 싱글톤이 보장되도록 해준다.

![[Pasted image 20240304184329.png|400]]

# @Configuration을 적용하지 않는다면?
`@Configuration`을 붙이면 바이트코드를 조작하는 CGLIB 기술을 사용해서 싱글톤을 보장하지만, 
만약 @Bean만 적용하면 CGLIB 기술이 싱글톤을 보장해주지 못한다.

