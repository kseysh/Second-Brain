# File system
Unix filesystem은 폴더와 파일의 계층 구조이다.
폴더도 파일이다. (모든 것이 파일이다.)

### Regular File
unix는 텍스트 파일 및 바이너리 파일을 모두 regular file로 취급하며, 구분하지 못한다.
### Directory file
다른 파일의 이름과 위치를 포함하는 파일
### 문자 특수 파일
터미널과 같은 문자 기반의 장치와 통신하는데 사용 됨
### 블록 특수 파일
주로 디스크 드라이브와 같은 블록 기반의 장치와 통신하는데 사용됨
### FIFO (pipe)
프로세스 간 통신을 위해 사용되는 파일 유형
하나의 프로세스가 파이프에서 데이터를 쓰면, 다른 프로세스가 파이프에서 그 데이터를 읽을 수 있다.
일종의 큐로 동작한다.
### Socket
프로세스 간의 네트워크 통신을 위해 사용되는 파일 유형
두 프로세스가 네트워크를 통해 데이터를 주고받을 수 있다.

![[Pasted image 20240909165159.png]]
맨 앞의 글자의 의미
- d: directory
- l: symbolic link
- b: block special file
- c: character special file
- p: FIFO
- -: regular file

