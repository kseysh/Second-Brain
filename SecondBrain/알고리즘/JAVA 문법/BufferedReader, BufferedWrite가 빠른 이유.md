## Scanner vs. BufferedReader
#### 버퍼 크기의 차이
Scanner: 1KB
BufferdReader: 8KB
이에 따라 I/O 호출이 Scanner보다 적게 발생함
#### 파싱 오버헤드
Scanner: nextInt, nextDouble등 데이터를 읽을 때, 정규식을 이용해서 파싱하는 오버헤드 발생
BufferedReader: nextLine으로 한 줄 단위로 가져옴
## System.out.println vs. BufferedWriter
#### 자동 Flush
println: 데이터 출력마다 버퍼를 비우고 출력장치로 데이터를 보냄
BufferedWriter: 데이터가 실제 출력장치로 가지 않고 버퍼에 쌓임, 버퍼가 가득 차거나 flush()를 호출했을 때만 출력 장치로 전송
#### 동기화 처리
System.out은 여러 스레드에서 동시에 접근해도 출력이 꼬이지 않도록 동기화 처리가 되어있다. 이 동기화 작업 자체가 오버헤드를 유발