
- app-prod DB 생성
- 특정 시간에 app-dev DB dump 파일 생성
- app-dev dump 파일 app-prod로 옮기기
- prod 환경에서 app-prod를 바라보도록 변경
- 특정 시간 이후에 app-dev에 생긴 쿼리문 app-prod에 옮기기
	- !주의! : update문의 순서 변경으로 잘못된 데이터로 

prod 환경에서 app-prod를 바라보도록 변경
dev 환경에서 app-dev를 바라보도록 변경
