## Array, Pointer and Structure
![[Pasted image 20231115151924.png]]

clockwise/spiral rule을 통해 identifier 의 타입을 쉽게 확정할 수 있다. 
1. identifier에서 시작 
2. 안에서 밖으로 시계 방향/나선으로 나간다 
3. element들을 만날 때 그들을 적절히 영어 문장으로 번역한다 >> rule의 원리는 연산자 우선순위에 기반한다! 
4. 시계 방향으로 돌리면서 나가보자. [ ] 연산자를 만나니까 array 
5. 다음 만나는 포인터를 통해 point
	\>\> foo is array of pointer

![[Pasted image 20231115152040.png]]
위는 array of pointer, 아래는 pointer to array
> clockwise/spiral rule에 너무 의존하지 말고 연산자 우선 순위를 기반으로 identifier를 결정하자

![[Pasted image 20231115152124.png]]
위의 p :  포인터이다. int constant를 가리키기에, 가리키는 대상의 값이 변할 수 없다. 
아래의 p : 포인터이다. 하지만 const pointer이기에 가리키는 대상이 변할 수 없다.

## Structure의 특징
![[Pasted image 20231115152231.png]]
struct의 기본적인 형태 
### 특징 
복합적인 데이터 타입들을 모을 수 있는 방법
struct 내부에 애들은 정의된 순서대로 작은 주소에서 높은 주소로 allocation된다.