	클라이언트가 선호하는 표현 요청
• Accept: 클라이언트가 선호하는 미디어 타입 전달
• Accept-Charset: 클라이언트가 선호하는 문자 인코딩
• Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
• Accept-Language: 클라이언트가 선호하는 자연 언어
• 협상 헤더는 요청시에만 사용
## 협상과 우선순위
### Quality Values(q) 값 사용
• 0~1, 클수록 높은 우선순위
• 생략하면 1
• Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
	• 1. ko-KR;q=1 (q생략)
	• 2. ko;q=0.9
	• 3. en-US;q=0.8
	• 4. en;q=0.7

### 구체적인 것이 우선한다.
• Accept: text/\*, text/plain, text/plain;format=flowed, \*/*
1. text/plain;format=flowed
2. text/plain
3. text/*
4. \*/*
### 구체적인 것을 기준으로 미디어 타입을 맞춘다.
• Accept: text/\*;q=0.3, text/html;q=0.7, text/html;level=1,
text/html;level=2;q=0.4, \*/\*;q=0.5
