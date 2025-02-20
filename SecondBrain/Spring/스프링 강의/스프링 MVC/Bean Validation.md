## @Valid
Java 표준 애노테이션으로 Spring뿐만 아니라 다른 프레임워크에서도 사용가능
객체 내부의 필드를 검증한다. (@Valid가 있는 필드도 함께 검증됨)
## @Validated
Spring에서만 동작하며, 메서드 파라미터와 반환 값의 검증도 지원한다.
그룹별 검증을 지원한다.
### 사용 시기
DTO나 엔티티 내부 필드를 검증할 때 -> @Valid
메서드 파라미터, 반환 값 검증이 필요할 때 -> @Validated
그룹별 검증이 필요할 때 -> @Validated

## 사용될 수 있는 이유
Spring Boot는 LocalValidatorFactoryBean을 자동으로 글로벌 Validator로 등록하는데, 이 때문에 @Valid나 @Validated를 사용하면 자동으로 이 Validator가 실행될 수 있다.

## Bean Validation이 기본으로 제공하는 오류 메시지 자세히 변경하기
![[Pasted image 20250220220829.png|400]]
## 특정 필드 예외가 아닌 전체 예외
만약, price 필드와 quantity 필드를 합쳐서 계산해서 예외를 발생시켜야 한다면, 자바 코드로 오류를 처리하는 것이 좋다.

# groups
동일한 모델 객체를 등록할 때와 수정할 때 다르게 검증하는 방법
![[Pasted image 20250220221634.png|300]]
이렇게 놓고, 검증 부분에 @NotNull(groups = {SaveCheck.class, UpdateCheck.class})처럼 둘 수 있다.
@Validated(SaveCheck.class) 이렇게 사용하면 특정 그룹만 검증할 수 있다.
=> 실무에서는 보통 update와 save의 전달 데이터가 달라 잘 사용하지 않는다.
내가 생각해봐도 요청 Request와 `~~Info` DTO는 분리하는게 더 깔끔할 듯! (request가 변경될 때, DTO는 변경되지 않을 수 있으므로)
