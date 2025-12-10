1. 리전 : 서울에다가 인스턴스 만들기
2. 인스턴스 연결 -> 연결로 들어가서 우분투 터미널 들어가기
3. `sudo apt-get update` -> `sudo apt-get upgrade` 입력
4. `sudo passwd root` 입력 후 최상위 루트 비밀번호 설정
5. `su` 입력 후 비밀번호를 누르면 root로 접속할 수 있다.
6. `sudo passwd ubuntu` 를 통해 우분투도 비밀번호를 설정해준다.
7. `sudo locale-gen ko_KR.UTF-8` 으로 지역설정
8. `sudo dpkg-reconfigure tzdata`로 지역설정, Asia -> Seoul로 설정하면 된다.
	1. a를 누르면 asia 주변으로 가고 asia 안에서 s를 누르면 서울 근처로 이동하여 방향키로 움직이고 엔터로 선택한다.
9. `sudo shutdown -r now` 를 통해 shutdown 실행
10. `sudo apt-get install -y nginx`를 통해 nginx 설치
11. 보안그룹 -> 인바운드 규칙 추가 -> 프로토콜 : HTTP/ 소스 유형 : Anywhere-IPv4 추가
12. `sudo apt-get install -y make build-essential libssl-dev zliblg-dev libbz2-dev \ (enter)`
13. `libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \ (enter)`
14. `xz-utils tk-dev`
15. `git clone https://github.com/pyenv/pyenv.git ~/.pyenv`
16. `sudo vi .bashrc` 로 시스템 환경변수를 설정해준다.
17. 맨 아래로 내려가서 i를 눌러준다 ( insert가 뜨며 입력모드로 변환된다. )
```
export PYENV_ROOT'[="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
if command -v pyenv 1>/dev/null 2>&1; then
		eval "$(pyenv init --path)"
fi
```
18. esc를 눌러 입력모드를 종료해준다 (insert가 사라진다.)
19. shift + :wq 로 작성을 완료해준다. :w => 쓰기 모드  :q => 종료
20. `source .bashrc`
21. `pyenv` 로 어떤 파이썬 라이브러리를 사용할 수 있는지 확인할 수 있다.
22. `pyenv 3.10.0` 으로 파이썬을 설치해준다.
