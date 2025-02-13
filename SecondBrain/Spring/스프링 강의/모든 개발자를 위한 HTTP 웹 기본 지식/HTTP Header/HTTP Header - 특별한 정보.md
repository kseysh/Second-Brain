# Host
요청한 호스트 정보(도메인)
• 요청에서 사용
• 필수
• 하나의 서버가 여러 도메인을 처리해야 할 때
• 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
# Location
페이지 리다이렉션
• 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동
(리다이렉트)
• 응답코드 3xx에서 설명
• 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
• 3xx (Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를
가리킴
# Allow
허용 가능한 HTTP 메서드
• 405 (Method Not Allowed) 에서 응답에 포함해야함
• Allow: GET, HEAD, PUT
