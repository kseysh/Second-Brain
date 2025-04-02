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
move $s0, HI와 같다.
HI를 이용해 32bit overflow를 확인할 수 있다
#### `mul rd, rs, rt`
least significant 32 bit를 rd로 옮긴다.