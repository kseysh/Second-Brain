## jps
자바 인스턴스 확인을 위해 사용한다.
- `-v` : 자바 옵션까지 포함하여 출력한다.
## jstat
GC 상황 확인을 위해 사용한다.
- `-gcutil` : 힙 영역의 사용량을 %로 보여준다.
- ![[Pasted image 20250304204045.png|300]]
	- S0, S1 : Survivor 영역
	- E : Eden 영역
	- YGC : Young 영역의 GC 횟수
	- YGCT : Young 영역의 GC가 수행된 누적 시간
	- O : Old 영역
	- OGCT : Old 영역의 GC가 수행된 누적 시간
	- FGC : Old 영역의 GC 횟수
	- FGCT : Old 영역의 GC가 수행된 누적 시간
	- 각 영역의 GC 시간  = XGCT / XGC
- `-gccapacity` : 각 영역에 할당되어 있는 메모리의 크기를 KB 단위로 나타낸다.
	- NGC : New 영역의 크기 관련 정보
	- OGC : Old 영역 크기 관련 정보
	- PGC : Perm 영역 크기 관련 정보
	- S0C, S1C : Survivor0, 1 영역에 현재 할당된 크기
	- EC : Eden 영역에 현재 할당된 크기
	- OC : Old 영역에 현재 할당된 크기
	- PC : Perm 영역에 현재 할당된 크기
	- YGC : Minor GC 횟수
	- FGC : Full GC 횟수
### jstatd (jstat daemon)
원격으로 JVM 상황을 모니터링하기 위해 사용
## verbosegc
자바 수행 시에 -verbosegc라는 옵션을 넣어주면 사용할 수 있다.
