Open Session In View의 약자로, view단까지 DB Connection을 열어두는 것
이에 따라 Transaction이 끝나도 DB Connection을 유지하게 된다.

#### OSIV 장점
Transaction 이후에도 Lazy Loading을 사용할 수 있다.

#### OSIV 단점
DB Connection이 View 렌더링까지 열려 있어 Connection pool 고갈 위험
서비스 계층과 View 계층의 결합
예측이 어려운 Lazy Loading 발생

### 끄는 방법
```yaml
jpa:
	open-in-view: false
```