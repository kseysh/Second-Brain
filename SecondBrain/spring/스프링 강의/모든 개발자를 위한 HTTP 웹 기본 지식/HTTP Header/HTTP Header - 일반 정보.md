- Referer: 이전 웹 페이지 주소
- User-Agent: 유저 에이전트 애플리케이션 정보
-  Server: 요청을 처리하는 오리진 서버의 소프트웨어 정보
- Date: 메시지가 생성된 날짜
# Referer
이전 웹 페이지 주소
• 현재 요청된 페이지의 이전 웹 페이지 주소
• A -> B로 이동하는 경우 B를 요청할 때 Referer: A 를 포함해서 요청
• Referer를 사용해서 유입 경로 분석 가능
• 요청에서 사용
## User-Agent
유저 에이전트 애플리케이션 정보
• user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/
537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
• 클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)
• 통계 정보
• 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
• 요청에서 사용
## Server
요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
• Server: Apache/2.2.22 (Debian)
• server: nginx
• 응답에서 사용
## Date
메시지가 발생한 날짜와 시간
• Date: Tue, 15 Nov 1994 08:12:31 GMT
• 응답에서 사용