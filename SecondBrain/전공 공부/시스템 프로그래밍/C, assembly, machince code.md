# Intel x86 Processors
수업에서 다루기 위해 한정한 프로세서 - x86-64 (64bit를 기반으로 한 시스템)
역사는 시험에 안 나옴

## Architecture (ISA : instruction set architecture)
기계에서 사용할 수 있는 명령어 세트 구조
ex) x86, x86-64, IA32
## Code forms
### Machine code
기계어 : 이진수로 이루어져 있음
### Assembly code
사람이 읽을 수 있는 형태로 텍스트로 변환된 형태
아키텍쳐에 따라 달라진다.
# Assembly/Machine Code View
assembly/machine code : 기본적으로 기계가 이해하기 쉬운 코드
기계가 사용할 수 있는 하드웨어의 자원을 통제하기 위해 만듦

![[Pasted image 20230926191549.png]]
CPU와 Memory의 관계를 책상과 책장의 관계로 보
## PC : Program counter
다음에 내가 실행해야할 instruction의 주소 (주소는 Memory안에 있음)
## Register file
연산에 필요한 데이터를 넣어놓는 공간 
ex) a와 b를 더할 때 a를 memory에서 꺼내고, 더하는 함수 꺼내고, b를 꺼내고 하지 않고 Register file에서 a와 b를 저장해놨다가 더하는 함수를 꺼내 더하는 방식으로 진행한다.
## Condition codes
이전에 수행했던 연산에 대한 결과 및 판단조건이 적혀있다.
## Memory
이외의 모든 데이터는 Memory에 저장된다.
Byte단위로 주소가 매겨진 큰 Array라고 할 수 있다.


## Assembly Cha





























