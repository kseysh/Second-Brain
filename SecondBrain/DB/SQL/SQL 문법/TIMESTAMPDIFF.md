MySQL에서 두 날짜간의 차이를 가져올 때 사용하는 함수

차이를 연, 분기, 월, 주, 일, 시, 분, 초를 지정하여 가져올 때 사용한다.
### TIMESTAMPDIFF : 시
#### 쿼리
`SELECT TIMESTAMPDIFF(HOUR, '2017-03-01', '2018-03-28');`
`-- SELECT TIMESTAMPDIFF(HOUR, '2017-03-01 00:00:00', '2018-03-28 00:00:00');`
#### 결과
9408
### TIMESTAMPDIFF : 일
#### 쿼리
`SELECT TIMESTAMPDIFF(DAY, '2017-03-01', '2018-03-28');`
`-- SELECT TIMESTAMPDIFF(DAY, '2017-03-01 00:00:00', '2018-03-28 00:00:00');`
#### 결과
392
### TIMESTAMPDIFF : 주
#### 쿼리
`SELECT TIMESTAMPDIFF(WEEK, '2017-03-01', '2018-03-28');`
`-- SELECT TIMESTAMPDIFF(WEEK, '2017-03-01 00:00:00', '2018-03-28 00:00:00');`
#### 결과
56
### TIMESTAMPDIFF : 월
#### 쿼리
`SELECT TIMESTAMPDIFF(MONTH, '2017-03-01', '2018-03-28');`
`-- SELECT TIMESTAMPDIFF(MONTH, '2017-03-01 00:00:00', '2018-03-28 00:00:00');`
#### 결과
12
### TIMESTAMPDIFF : 분기
#### 쿼리
`SELECT TIMESTAMPDIFF(QUARTER, '2017-03-01', '2018-03-28');`
`-- SELECT TIMESTAMPDIFF(QUARTER, '2017-03-01 00:00:00', '2018-03-28 00:00:00');`
#### 결과
4
### TIMESTAMPDIFF : 연
#### 쿼리
`SELECT TIMESTAMPDIFF(YEAR, '2017-03-01', '2018-03-28');`
`-- SELECT TIMESTAMPDIFF(YEAR, '2017-03-01 00:00:00', '2018-03-28 00:00:00');`
#### 결과
1