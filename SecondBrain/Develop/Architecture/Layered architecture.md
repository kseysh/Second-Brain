# Layer
관심사의 집합으로 따로 각각의 집합이라고 표현하지 않고 Layer라고 표현하며 계층관계를 나타내는 것은 , 상위 Layer에서 하위Layer로만 컨트롤 할 수 있기 때문이다.
# Layer로 나누는 이유
한 Layer가 변한다고, 다른 Layer가 변하지 않도록 하기 위함 (3-Layer 재사용성)

## Presentation Layer
### 책임
사용자의 요청을 받아서 응답을 해주는 역할을 한다. 
사용자 요청이 잘못된 경우 exception 처리를 해준다.
### 구현
해당 layer에서는 facade layer 혹은 application layer를 호출하여 사용자의 요청에 대한 응답을 해준다. 해당 layer에서는 response를 위한 dto를 정의하고, request를 위한 dto를 정의한다. 또한 response를 만드는 역할을 한다.
(Controller?)
## Domain Layer
### 책임
domain layer는 entity와 entity의 관계를 정의한다.

## Interface Layer
### 책임
interface layer는 외부와의 통신을 위한 interface를 정의한다. 
### 구현
repository interface가 해당된다.

## Application Layer
### 책임
application layer는 비즈니스 로직을 정의한다. 
transaction 처리를 한다.
### 구현
해당 layer에서는 *service를 정의*한다. 해당 layer에서는 entity를 dto로 변환하는 역할을 한다. 

##  Facade Layer
[[Fa]]
facade layer는 application layer를 호출하는 역할을 한다. 해당 layer에서는 service의 orchestrator 역할을 한다.