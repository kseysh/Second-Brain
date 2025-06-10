## MS-DOS
simple Structure로 이루어져 있다.
모놀리식 구조이다.
## Initial UNIX
System programs와 Kernel로 이루어져 있다.
### 모놀리식의 단점
한 가지만 변경하더라도 전부를 컴파일해야한다.
하나가 오류가 나더라도 모두가 죽는다.
## Layered-Approach
![[Pasted image 20250311172954.png|150]]
가장 안쪽은 하드웨어
가장 바깥쪽은 유저 인터페이스이다.
Layer간을 뛰어넘지 않고, 한 단계씩에만 영향을 끼칠 수 있도록 한다.
## Microkernel (Mach)
![[Pasted image 20250311173256.png|300]]
중요하지 않은 컴포넌트는 커널에서 다 빼고 유저 레벨 프로그램에서 작성하도록 하는 방법
message 전달을 통해 사용자 모듈간 통신을 한다.
### 장점
- Kernel size가 작아진다
- 운영체제를 새로운 아키텍처로 이식하기 쉽다
- 커널 모드에서 실행되는 코드가 줄어들어 안정적이다
- 보안성이 향상된다.
### 단점
- user space와 kernel space간의 통신으로 인해 성능 오버헤드가 발생한다.
## Modules
많은 현대 OS에서는 loadable kernel modules(LKM, 링킹을 나중에 하므로 메모리의 크기가 줄일 수 있다)를 사용한다.
## Hybrid Systems
요즘은 하나의 모델만을 사용하는 것이 아니라 여러가지를 섞어서 사용한다.
방법론의 장단점 정도를 알아두기
여기까지는 개요. . . .