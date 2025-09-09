System.out.println은 표준 출력으로 출력하기 위해 바로 표준 출력 스트림에 flush를 적용한다.
따라서 System.out.println을 실행할 때마다 kernel 환경에 접근하는 OS I/O가 발생하여 비교적 느리다.

하지만, log.info 같은 경우에는 내부적으로 버퍼링을 지원하기 때문에 log.info를 실행할때마다 즉시 flush를 하지 않아 OS I/O가 자주 발생하지 않는 이점이 있다.

또한, 추가적으로 log.info의 경우 로그 메시지 단위 atomic을 보장한다, 하지만 System.out.println의 경우에는 줄 단위로 synchronized가 적용되어 있어, System.out.println끼리의 로그 출력이 섞일 수 있다.