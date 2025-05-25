## 함수적 종속성의 폐쇄(Closure of Functional Dependencies)
•	함수적 종속성 집합 F가 주어졌을 때, F로부터 논리적으로 유도 가능한 모든 함수적 종속성이 존재한다.
#### example
A → B, B → C가 주어지면, A → C를 유도할 수 있다.
•	이렇게 유도된 모든 함수적 종속성의 집합을 F의 폐쇄, 즉 F<sup>+</sup>라고 한다
## 함수적 종속성의 폐쇄 (Closure of Functional Dependencies)
•	우리는 Armstrong의 공리를 반복적으로 적용하여 F의 폐쇄 F⁺를 구할 수 있다:

Armstrong의 공리:
•	만약 β ⊆ α 이면, α → β (반사성 reflexivity)
•	만약 α → β 이고 γ가 존재하면, γα → γβ (확장성 augmentation)
•	만약 α → β이고, β → γ이면, α → γ (추이성 transitivity)
•	이 규칙들은 다음과 같은 성질을 가진다:
•	sound (실제로 성립하는 함수적 종속성만 생성함)
•	complete (성립하는 모든 함수적 종속성을 생성할 수 있음)
#### example
•	R = (A, B, C, G, H, I)
•	F = {
	A → B
	A → C
	CG → H
	CG → I
	B → H}
•	F⁺에 속하는 몇 가지 종속성:
•	A → H
	•	A → B 그리고 B → H 의 추이성(transitivity)을 통해 유도됨
•	AG → I
	•	A → C를 G로 확장하여 AG → CG를 얻고(확장성), CG → I의 추이성을 통해 유도됨
•	CG → HI
	•	CG → I를 확장하여 CGI → I를 얻고(확장성), CG → H도 확장하여 CGI → H를 얻은 후(확장성), 추이성을 통해 CGI → HI 유도됨

## F⁺ 계산 절차
•	주어진 함수 종속성 집합 F의 클로저 F⁺를 계산하는 방법:
F⁺ = F
```c
repeat
	for each functional dependency f in F⁺
		apply reflexivity and augmentation rules on f
		add the resulting functional dependencies to F⁺
	for each pair of functional dependencies f₁and f₂ in F⁺
		if f₁and f₂ can be combined using transitivity
			then add the resulting functional dependency to F⁺
until F⁺ does not change any further
```

## 함수 종속성 클로저 (Closure of Functional Dependencies)
•	추가 규칙:
•	α → β이고 α → γ이면, α → βγ이다 (union)
•	α → βγ이면, α → β이고 α → γ이다 (decomposition)
•	α → β이고 γβ → δ이면, αγ → δ이다 (pseudotransitivity)

위 규칙들은 Armstrong의 공리(Armstrong’s axioms)로부터 유도할 수 있다.
## 속성 집합 클로저 (Closure of Attribute Sets)
•	속성 집합 α가 주어졌을 때, F에 대해 α의 클로저(closure)를 α⁺로 정의한다.
α⁺는 α에 의해 F 하에서 함수적으로 결정되는 속성들의 집합이다.
•	α⁺를 계산하는 알고리즘:

```c
result := α;
while (result에 변화가 있을 동안) do
    F의 각 β → γ에 대해
        if β ⊆ result이면
            result := result ∪ γ
end
```
## Example of Attribute Set Closure
•	R = (A, B, C, G, H, I)
•	F = {
	A → B
	A → C
	CG → H
	CG → I
	B → H
	}
#### (AG)⁺ 계산 과정:
1.	result = AG
2.	result = ABCG (A → C와 A → B를 적용)
3.	result = ABCGH (CG → H, 그리고 CG ⊆ AGBC)
4.	result = ABCGHI (CG → I, 그리고 CG ⊆ AGBCH)
#### AG가 후보 키(candidate key)인가?
1.	AG가 슈퍼 키(super key)인가?
	1.	AG → R인가? == (AG)⁺ ⊇ R 인가? (맞음)
2.	AG의 부분집합이 슈퍼 키인가?
	1.	A → R인가? == (A)⁺ ⊇ R 인가? (아님)
	2.	G → R인가? == (G)⁺ ⊇ R 인가? (아님)
=> 따라서 후보키가 맞음
## 속성 클로저의 활용
•	superkey 테스트:
	•	α⁺를 계산해서, α⁺가 R의 모든 속성을 포함하는지 확인
•	함수 종속성 테스트:
	•	α → β가 F⁺에 포함되는지 확인하려면, α⁺를 계산하고 β ⊆ α⁺인지 확인
	• α⁺: F로 부터 α로 추론 가능한 모든 속성들의 집합
	• α → β가 F⁺에 포함되기 위해서는 β의 모든 속성이 α로부터 유도 가능해야 함
	•	이 방법은 간단하고 효율적
•	F의 Closure 계산:
	•	R의 모든 부분집합 γ에 대해 γ⁺를 계산하고, γ⁺의 모든 부분집합 S에 대해 γ → S 형태의 함수 종속성을 출력
## Lossless-join Decomposition
•	R = (R₁, R₂)인 경우, 가능한 모든 관계 r에 대해
	•	r = πR₁(r) ⨝ πR₂(r)가 되어야 한다.
•	R을 R₁과 R₂로 분해했을 때 다음 중 하나라도 F⁺에 포함되면 Lossless-join Decomposition이다:
	•	R₁ ∩ R₂ → R₁
	•	R₁ ∩ R₂ → R₂
•	즉, R₁ ∩ R₂가 R₁ 또는 R₂의 슈퍼키이면 Lossless-join Decomposition가 된다.
#### 예제
•	R = (A, B, C)
•	F = {A → B, B → C}
1. 분해 방법:
	•	R₁ = (A, B), R₂ = (B, C)
	•	Lossless Join
	•	R₁ ∩ R₂ = {B}, B → BC

2. 또 다른 분해 방법:
	•	R₁ = (A, B), R₂ = (A, C)
	•	Lossless Join이지만
	•	Dependency preserving이 아님 (B → C를 R₁, R₂만으로 검사할 수 없음)
## Dependency Preservation
•	Fi: Ri에만 포함된 속성으로 이루어진 F⁺의 부분집합
	•	분해가 종속성 보존이면:
	•	(F₁ ∪ F₂ ∪ … ∪ Fn)⁺ = F⁺
	•	그렇지 않으면, 종속성 위반 검사를 위해 조인을 해야 해서 비용이 크다.
#### 예제
•	R = (A, B, C)
•	F = {A → B, B → C}
•	Key = {A}
•	R은 BCNF가 아님
•	R₁ = (A, B), R₂ = (B, C)로 분해
	•	R₁, R₂는 BCNF 만족
	•	Lossless-join
	•	Dependency Preservation
## BCNF와 종속성 보존
•	항상 3NF로 Lossless-join, 종속성 보존이 가능하다.
•	항상 BCNF로 Lossless-join은 가능하지만, Dependency Preservation은 항상 가능하지는 않다.

예시:
	•	R = (J, K, L)
	•	F = {JK → L, L → K}
	•	후보 키 = JK, JL
	•	R은 BCNF가 아님
	•	어떤 분해를 해도 JK → L 종속성을 보존할 수 없음
	•	JK → L 검사를 위해 조인이 필요하다.
## BCNF와 3NF 비교
•	3NF 분해:
	•	Lossless-join 가능
	•	Dependency Preservation 가능
•	BCNF 분해:
	•	Lossless-join 가능
	•	Dependency Preservation은 불확실
## 설계 목표 (Design Goals)
관계형 데이터베이스 설계의 목표:
	•	BCNF 만족
	•	Lossless-join
	•	Dependency Preservation

이를 모두 달성할 수 없다면:
	•	Dependency Preservation 포기
	•	3NF 사용에 따른 중복 허용

주의:
	•	SQL은 슈퍼키 외에는 직접적으로 함수 종속성(FD)을 지정하는 방법을 제공하지 않는다.
	•	Assertion을 통해 FD를 명시할 수는 있지만, 비용이 많이 들고 (게다가 대부분의 데이터베이스 시스템은 지원하지 않음)
	•	설령 종속성 보존 분해를 했더라도, SQL만으로 키가 아닌 속성 집합에 대한 종속성 검증은 쉽지 않다.