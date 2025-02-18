@Controller는 반환 값이 String이면 뷰 이름으로 인식되어 뷰를 찾고 뷰가 렌더링 된다.
@RestController는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 바디에 바로 입력한다.