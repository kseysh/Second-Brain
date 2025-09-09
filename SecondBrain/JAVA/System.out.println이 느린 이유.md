System.out.println은 표준 출력으로 출력하기 위해 바로 표준 출력 스트림에 flush를 적용한다.
따라서 System.out.println을 실행할 때마다 kernel 환경에 접근하는 OS I/O가 발생하여 비교적 느리다.

하지만, log.info 같은 경우에는 버퍼링을 진행하기 때문에 