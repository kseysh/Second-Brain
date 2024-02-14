2. `@RequestBody` 어노테이션:
    - `@RequestBody` 어노테이션은 클라이언트가 HTTP 요청의 본문으로 보낸 데이터를 컨트롤러 메서드의 매개변수에 바인딩한다.
    - 주로 POST 또는 PUT 요청에서 클라이언트가 JSON데이터를 전송하고 이 데이터를 서버에서 처리해야 할 때 사용된다.
    - 스프링은 HTTP 요청의 본문을 자바 객체로 역직렬화하고, 이를 컨트롤러 메서드에 전달한다.

예시:
```
@PostMapping("/saveData") 
public ResponseEntity<String> saveData(@RequestBody MyData data) {     
	// 클라이언트에서 전달한 JSON 데이터를 MyData 객체로 변환하여 처리     
	// ...     
	return ResponseEntity.ok("Data saved successfully"); 
}
```