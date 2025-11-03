## Scanner vs. BufferedReader
#### 버퍼 크기의 차이
Scanner: 1KB
BufferdReader: 8KB
이에 따라 I/O 호출이 Scanner보다 적게 발생함
#### 파싱 오버헤드
Scanner: nextInt, nextDouble등 데이터를 읽을 때, 정규식을 이용해서 파싱하는 오버헤드 발생
BufferedReader: nextLine으로 한 줄 단위로 가져옴
## S