---
sticker: emoji//270f-fe0f
---
<div style="text-align: center; font-size:30;font-weight:bold ">시스템 프로그래밍 HW2 과제</div>

<div style="text-align: right">컴퓨터공학과 12201709 김승환</div>

# 1. `printf()`함수 코드 설명
`printf()`함수는 compile Time interpositioning과 Link-time interpositioning의 코드가 거의 비슷하므로 `printf()` 함수는 compile Time interpositioning을 기준으로 설명하되, 차이점에 대해서는 Interpositioning 수행 과정 및 결과 설명에서 따로 설명하도록 하겠습니다.

## 1-1. 구현 코드


## 1-2. 코드 설명
모든 입력 값에 대한 출력은 format 인식 기능이 없는 출력 함수인 putchar를 통해 출력되도록 하였습니다. 또한 `printf()`의 원형이 `int printf(const char* format, ...)`이므로
