## Agenda
• 배경
• 연속 메모리 할당
• 세그멘테이션
• 페이징
• 사례 연구
## Background
![[Pasted image 20250520164517.png|500]]
• 프로그램은 실행되기 위해 디스크에서 메모리로 로드되어 프로세스 내에 배치되어야 함
• 메인 메모리와 레지스터는 CPU가 직접 접근할 수 있는 유일한 저장 장치임
• 메모리 장치는 주소와 읽기 요청, 또는 주소와 데이터 및 쓰기 요청의 연속만을 인식함
• 레지스터 접근은 한 CPU 클럭(또는 그 이하) 소요
• 메인 메모리는 여러 클럭 사이클이 걸릴 수 있어 stall 이 발생함
• 캐시는 메인 메모리와 CPU 레지스터 사이에 위치
• 올바른 동작을 보장하기 위해 메모리 보호가 필요함
## Base and Limit Registers
• *base*(해당 프로세스의 memory 시작 주소)와 *limit*(해당 프로세스가 할당받은 memory 크기) 레지스터 한 쌍은 논리 주소 공간을 정의함
• CPU는 사용자 모드에서 발생하는 모든 메모리 접근이 해당 사용자에 대해 베이스와 리미트 사이에 있는지 확인해야 함
![[Pasted image 20250520164647.png|150]]
## Hardware Address Protection
![[Pasted image 20250520164706.png|300]]
## Address Binding
• 프로그램의 생애 주기 단계마다 주소는 다른 방식으로 표현됨
	• source code address는 일반적으로 symbolic임
	• Compiled code addresses는 relocatable address에 바인딩됨
		• 예: “이 모듈의 시작점에서 14바이트 떨어진 위치”
	• linker나 loader는 relocatable address를 절대 주소로 바인딩함
		• 예: 74014
	• 각 바인딩은 하나의 주소 공간을 다른 주소 공간에 매핑함
![[Pasted image 20250520164737.png|150]]
## Logical vs. Physical Address Space
• *Logical address* - CPU가 생성한 주소, *가상 주소*라고도 함 (Program에 명시된 주소)
• *Physical address* - 메모리 장치가 보는 주소
• 논리 주소 공간은 프로그램이 생성할 수 있는 모든 논리 주소들의 집합
• 물리 주소 공간은 프로그램이 생성할 수 있는 모든 물리 주소들의 집합
## Memory-Management Unit (MMU)
• 실행 시간에 logical address를 physical address로 매핑하는 하드웨어 장치
• 다양한 방법이 있음
• 먼저, relocation register를 사용하는 단순한 방식 고려
• physical address = logical address + relocation address의 값
• MS-DOS는 Intel 80x86에서 4개의 리로케이션 레지스터를 사용함
![[Pasted image 20250520165929.png|300]]
MMU는 HW임. (logical address에 relocation register에 있는 값을 더해 physical address로 변환하는 일을 한다.)