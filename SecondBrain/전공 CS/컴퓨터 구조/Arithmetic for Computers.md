## Addition
unsigned: overflow check를 안 함
signed: overflow가 일어나면 부호가 바뀜
## Multiplication
![[Pasted image 20250402172302.png|300]]
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
bias: 127
### Double-Precision Range
![[Pasted image 20250403140051.png|200]]
Exponent 000...00 과 111...11은 Reserved value (따라서 smallest/largest value 계산 시 조심)
bias: 1023
### Floating Point Precision
