1. `/` 는 계층 관계를 표현합니다.
2. `/` 는 URI 마지막에 포함하지 않습니다.
    - `https://sopt.org` ( O )
    - `https://sopt.org/` ( X )
3. 가독성을 높이기 위해 불가피한 경우에는 `-` 를 사용합니다.
    - `https://sopt.org/server-part` ( O )
    - `https://sopt.org/serverpart` ( X )
4. 언더바(`_`)는 사용하지 않습니다.
    - `https://sopt.org/server-part` ( O )
    - `https://sopt.org/server_part` ( X )
5. 소문자를 사용하는 것이 적합합니다.
    - `https://sopt.org/server-part` ( O )
    - `https://sopt.org/serverPart` ( X )
6. 파일 확장자를 포함시키지 않습니다.
    - `https://sopt.org/server.png` ( X )
7. 리소스 간의 연간 관계를 표현합니다.
    - `https://sopt.org/members/{memberId}/device`
8. (참고) 계층 구조를 사용할 경우에 상위 구조는 collection으로 보고 복수형으로 사용