## Addition
unsigned: overflow check를 안 함
signed: overflow가 일어나면 부호가 바뀜
## Multiplication
![[Pasted image 20250402172302.png|300]]
![[Pasted image 20250414130133.png|300]]
이런 느낌임
![[Pasted image 20250402172323.png|400]]
#### example
![[Pasted image 20250402172351.png|400]]
### Optimized Multiplier
add와 shift가 병렬적으로 실행된다.
partial-product addition 당 한 사이클
![[Pasted image 20250402172421.png|400]]
### Faster Multiplier
여러개의 adder를 사용한다.
pipeline화 할 수 있다.
여러개의 곱셈을 병렬적으로 실행할 수 있다
![[Pasted image 20250402172648.png|400]]
### MIPS Multiplication
32bit x 32bit를 할 때, 저장하는 레지스터
- HI - most significant 32 bits
- LO - least significant 32 bits
#### `mult rs, rt / multu rs, rt`
rs x rt를 해서, 상위 32bit는 HI, 하위 32bit는 LO에 저장함
#### `mfhi rd / mflo rd`
move $s0, HI와 같다. (move from HI/LO)
HI를 이용해 32bit overflow를 확인할 수 있다
#### `mul rd, rs, rt`
least significant 32 bit를 rd로 옮긴다.
아래 두 개와 같은 역할을 함
mult rs rt
mflo $rd
## Division
5 / 3일 때, 
divisor: 3
dividend: 5
- 0으로 나누는 경우를 먼저 확인
- Long division
	- If divisor ≤ dividend bits
		- 몫에 1을 넣고, 뺄셈 수행
	- else
		- 몫에 0을 넣고, 다음 나눠지는 수의 비트를 내려옴
- restoring division
	- 이걸 사용
	- 일단 뺀 후, 나머지가 0보다 작아지면 나누는 수를 다시 더함
- signed division
	- 절댓값을 이용해 나눈 후 몫과 나머지의 부호를 필요에 따라 조정
### Division Hardware
![[Pasted image 20250403134535.png|300]]
#### example
7 / 2
![[Pasted image 20250403134631.png|300]]
Remainder가 양수라면 (= 나머지보다 제수가 작다면) 다시 더해주지 않고 몫에 1을 더한다.
이 때, 몫은 왼쪽으로 shift, Divisor는 오른쪽으로 shift를 진행하므로 optimized할 수 있다.
### Optimized Divider
![[Pasted image 20250403134927.png|300]]
One cycle per partial-remainder subtraction
multiplier와 같은 hardware를 사용할 수 있다.
#### example
![[Pasted image 20250403135127.png|300]]
### Faster Division
multiplier처럼 병렬 계산은 사용하기 어렵다.
- 이전 결과에 의해 영향을 받기 때문
## MIPS Division
결과를 위해 HI/LO register를 사용한다.
- HI: 32-bit remainder
- LO: 32-bit quotient
### `div rs, rt / divu rs, rt`
0 나눗셈을 체크하지 않음 (소프트웨어가 따로 처리해야 함)
결과에 접근하기 위해 mfhi, mflo를 사용한다.
## Floating Point
- Single precision : 32-bit
- Double precision : 64-bit
![[Pasted image 20250403135708.png|300]]
### Single-Precision Range
![[Pasted image 20250403140039.png|200]]
Exponent 00000000 과 11111111은 Reserved value (따라서 smallest/largest value 계산 시 조심)
Exponent는 8자리
bias: 127
### Double-Precision Range
![[Pasted image 20250403140051.png|200]]
Exponent 000...00 과 111...11은 Reserved value (따라서 smallest/largest value 계산 시 조심)
Exponent는 11자리
bias: 1023
### Floating Point Precision
- single: 약 2<sup>-23</sup>의 정밀도 
	- 이는 10진수로 약 6자리 정도의 정밀도를 뜻함
- Double: 약 2<sup>-52</sup>의 정밀도
	- 이는 10진수로 약 16자리 정도의 정밀도를 뜻함
#### example
-0.75를 single과 double로 표현
![[Pasted image 20250403140721.png|200]]

1/10000001/010000...00을 숫자로
![[Pasted image 20250403140806.png|200]]

![[Pasted image 20250403140837.png|300]]
single은 2<sup>-23</sup>의 정밀도를 가지는데, 여기서는 Fraction이 30bit이므로 7bit가 사라지게된다.
## Denormal Numbers
### Exponent = 00...000
Fraction 앞에 원래는 1로 간주되지만, Exp가 00...000이면 0으로 간주됨
![[Pasted image 20250408210443.png|200]]
![[Pasted image 20250408210456.png|200]]
### Exponent = 11...111
#### Fraction = 000...0
Infinity 표시
이후 계산에 사용 가능, overflow 검사 필요 없음
#### Fraction != 000...0
NaN
잘못된 또는 정의되지 않은 결과를 나타냄
이후 계산에 계속 사용 가능
## Floating Point Addition
• 4자리 10진수 예제 고려:
	•	9.999 × 10¹ + 1.610 × 10⁻¹
1. 소수점 정렬
	•	지수가 더 작은 수를 이동시킴
	•	9.999 × 10¹ + 0.016 × 10¹
2. 가수 덧셈
	•	9.999 × 10¹ + 0.016 × 10¹ = 10.015 × 10¹
3. 결과 정규화 및 오버/언더플로 검사
	•	1.0015 × 10²
4. 반올림 및 필요 시 재정규화
	•	1.002 × 10²
## Floating Point Addition
• 4자리 이진수 예제:
	•	1.000₂ × 2⁻¹ + –1.110₂ × 2⁻² (즉, 0.5 + –0.4375)
1. 이진 소수점 정렬
	•	지수가 더 작은 수를 이동시킴
	•	1.000₂ × 2⁻¹ + –0.111₂ × 2⁻¹
2. 가수 덧셈
	•	1.000₂ × 2⁻¹ + –0.111₂ × 2⁻¹ = 0.001₂ × 2⁻¹
3. 결과 정규화 및 오버/언더플로 검사
	•	1.000₂ × 2⁻⁴ (오버/언더플로 없음)
4. 반올림 및 필요 시 재정규화
	•	1.000₂ × 2⁻⁴ (변화 없음) = 0.0625
## FP Adder Hardware
• 정수 가산기보다 훨씬 복잡함
• 한 클럭 사이클 안에 처리하기엔 시간이 너무 오래 걸림
	• 정수 연산보다 훨씬 오래 걸림
	• 클럭 속도를 낮추면 모든 명령어에 불이익 발생
• 부동소수점 가산기는 일반적으로 여러 사이클에 걸쳐 수행됨
• 파이프라인 처리가 가능함

회로 그릴 필요 X
하드웨어 이해 O
왜 이렇게 생겼는지, 왜 오래 걸리는지만 알기
![[Pasted image 20250408213909.png|400]]

![[Pasted image 20250410195114.png|400]]
small ALU를 이용해 누가 얼마나 더 큰지 비교한다

![[Pasted image 20250410195126.png|400]]
작은 값을 큰 값에 맞춰서 Fraction을 shift한다.
Control이 왼쪽, 오른쪽 중 무엇을 shift할지 결정한다
왼쪽 MUX와 오른쪽 MUX는 서로 다른 값을 가진다
그러면 왼쪽은 작은 exp를 가진 Fraction이 내려오고, 오른쪽은 큰 exp를 가진 Fraction이 내려온다.

![[Pasted image 20250410195136.png|400]]


![[Pasted image 20250410195144.png|400]]
Big ALU를 이용해 두 값을 더하고, 정규식과 맞는지 확인하고 shift 해야하면 shift하고 exponent를 증가/감소 시킨다.
![[Pasted image 20250410195153.png|400]]
반올림 하는 하드웨어가 들어가서 Fraction을 보고 자릿수를 넘으면 반올림을 한다.
유효숫자 바깥으로 나간 data를 반올림한다

![[Pasted image 20250410195203.png|400]]
![[Pasted image 20250410203739.png|100]] 이 사진과 같은 경우 정규표현식에 맞지 않기 때문에 반올림까지 한 후 다시 정규표현식에 맞도록 해야함

## Floating-Point Multiplication
### 4자리 10진수 예제
•	1.110 × 10¹⁰ × 9.200 × 10⁻⁵
1.	지수끼리 더함
	•	편향 지수일 경우, 합에서 편향 값을 뺌
	•	새로운 지수 = 10 + (–5) = 5
2.	가수(significand)를 곱함
	•	1.110 × 9.200 = 10.212 → 10.212 × 10⁵
3.	결과를 정규화하고 오버플로/언더플로 여부 확인
	•	1.0212 × 10⁶
4.	반올림하고 필요 시 다시 정규화
	•	1.021 × 10⁶
5.	부호 결정: 피연산자의 부호에 따라 결정
	•	+1.021 × 10⁶

### 4자리 2진수 예제
1.000₂ × 2⁻¹ × –1.110₂ × 2⁻² (0.5 × –0.4375)
1.	지수 더하기
	•	편향되지 않은 경우: –1 + (–2) = –3
	•	편향된 경우: (–1 + 127) + (–2 + 127) = –3 + 254 – 127 = –3 + 127
2.	가수 곱하기
	•	1.000₂ × 1.110₂ = 1.110₂ → 1.110₂ × 2⁻³
3.	결과 정규화 및 오버/언더플로 확인
	•	1.110₂ × 2⁻³ (변화 없음, 오버/언더플로 없음)
4.	반올림 및 필요 시 재정규화
	•	1.110₂ × 2⁻³ (변화 없음)
5.	부호 결정: 양수 × 음수 → 음수
	•	–1.110₂ × 2⁻³ = –0.21875
## 부동소수점 산술 연산 하드웨어
•	부동소수점 곱셈기는 덧셈기와 비슷한 복잡도를 가짐
	•	그러나 가수 연산 시 덧셈기 대신 곱셈기를 사용
•	일반적으로 수행되는 연산들:
	•	덧셈, 뺄셈, 곱셈, 나눗셈, 역수, 제곱근
	•	부동소수점 ↔ 정수 변환
•	대부분의 연산은 여러 클럭 사이클 소요
	•	파이프라인화 가능
## MIPS의 부동소수점 명령어
•	부동소수점 연산 하드웨어는 코프로세서 1 (Coprocessor 1)
	•	명령어 집합 구조(ISA)를 확장하는 보조 프로세서
•	별도의 부동소수점 레지스터 존재
	•	단정도(single-precision) 32개: $f0, $f1, … $f31
	•	배정도(double-precision)일 경우 쌍으로 사용: $f0/$f1, $f2/$f3, …
		•	MIPS ISA 버전 2는 64비트 부동소수점 레지스터 32개 지원
•	부동소수점 명령어는 오직 부동소수점 레지스터에만 작용
	•	일반적으로 FP 데이터에 정수 연산을 하거나 그 반대는 하지 않음
	•	더 많은 레지스터를 사용하면서도 코드 크기 영향은 최소화
•	부동소수점 로드/스토어 명령어
	•	lwc1, ldc1, swc1, sdc1
	•	예: `ldc1 $f8, 32($sp)`

• Single-precision arithmetic
	•	add.s, sub.s, mul.s, div.s
		•	예: add.s $f0, $f1, $f6
• Double-precision arithmetic
	•	add.d, sub.d, mul.d, div.d
		•	예: mul.d $f4, $f4, $f6
• Single- and double-precision comparison
	•	c.xx.s, c.xx.d (xx는 eq, lt, le 등)
	•	부동소수점 조건 코드 비트 설정 또는 해제
		•	예: c.lt.s $f3, $f4
• Branch on FP condition code true or false
	•	bc1t, bc1f
		•	예: bc1t TargetLabel
### 부동소수점 예제: 화씨(°F) → 섭씨(°C) 변환
```c
float f2c (float fahr) {
  return ((5.0 / 9.0) * (fahr - 32.0));
}
```
•	fahr는 $f12, 결과는 $f0, 상수는 전역 메모리 영역에 저장
•	컴파일된 MIPS 코드:
```
f2c:
  lwc1 $f16, const5($gp)
  lwc1 $f18, const9($gp)
  div.s $f16, $f16, $f18
  lwc1 $f18, const32($gp)
  sub.s $f18, $f12, $f18
  mul.s $f0, $f16, $f18
  jr $ra
```

### FP Example: Array Multiplication
```c
C code:
void mm (double x[][], double y[][], double z[][]) {
	int i, j, k;
	for (i = 0; i! = 32; i = i + 1)
		for (j = 0; j! = 32; j = j + 1)
			for (k = 0; k! = 32; k = k + 1)
				x[i][j] = x[i][j]+ y[i][k] * z[k][j];
}
```
x, y, z $a0, $a1, $a2
i, j, k $s0, $s1, $s2

![[Pasted image 20250410210309.png|300]]
li (load immediate)
$f4 / $f5 : x\[i]\[j]
$f16 / $f17 : z\[k]\[j]
![[Pasted image 20250410210319.png|300]]
$f18 / $f19 : y\[i]\[k]

## 정확한 산술 연산
•	IEEE 표준 754는 추가적인 반올림 제어를 명시함
	•	추가 정밀도를 위한 비트 사용 (가드 비트, 반올림 비트, 스티키 비트)
	![[Pasted image 20250410212626.png|200]]
	•	반올림 모드 선택 가능
	•	프로그래머가 계산의 수치적 동작을 세밀하게 조정할 수 있도록 함
•	모든 부동소수점(FP) 유닛이 모든 옵션을 구현하는 것은 아님
	•	대부분의 프로그래밍 언어와 부동소수점 라이브러리는 기본값만 사용
•	하드웨어 복잡도, 성능, 시장 요구 사이의 트레이드오프 존재
![[Pasted image 20250410212756.png|500]]
반올림: 냅두거나 더하거나
## 서브워드 병렬 처리
•	그래픽 및 오디오 응용 프로그램은 짧은 벡터에 대한 동시 연산 수행을 통해 이점을 얻을 수 있음
	•	예: 128비트 덧셈기
		•	8비트 덧셈 16개
		•	16비트 덧셈 8개
		•	32비트 덧셈 4개
•	데이터 수준 병렬 처리, 벡터 병렬 처리, 혹은 단일 명령어 다중 데이터(SIMD)라고도 함
## 스트리밍 SIMD 확장 2 (SSE2)
•	128비트 레지스터 4개 추가
	•	AMD64/EM64T에서는 8개로 확장됨
•	여러 개의 부동소수점 피연산자 처리 가능
• 2 × 64-bit double precision
• 4 × 32-bit double precision
•	명령어들이 동시에 이들에 작용함
	•	단일 명령어 다중 데이터(SIMD) 방식
## 고급 벡터 확장 (AVX)
•	AVX-512
•	512비트 레지스터 32개 추가
•	여러 개의 부동소수점 피연산자 처리 가능
	• 8 × 64-bit double precision
	• 16 × 32-bit double precision
## Right Shift and Division
i비트만큼 왼쪽 시프트하면 정수를 2<sup>i</sup>만큼 곱하는 것과 같다.
오른쪽 시프트는 2<sup>i</sup>로 나누는 것이지만, 부호 없는 정수에만 해당된다.
## Associativity
•	병렬 프로그램은 연산을 예상치 못한 순서로 섞어서 실행할 수 있음
•	결합법칙(associativity)을 당연히 여기는 가정이 실패할 수 있음
•	병렬성의 정도가 달라질 때 프로그램을 검증할 필요가 있음
![[Pasted image 20250413170814.png|300]]
