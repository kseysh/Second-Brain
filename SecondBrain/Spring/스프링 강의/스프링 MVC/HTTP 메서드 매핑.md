## `@RequestMapping`
- 대부분의 소겅을 배열로 제공하여 매핑 다중 설정이 가능하다. ex) `{"/hello-basic", "/hello-go"}`
- 특정 헤더 조건을 매핑할 수 있다. ex) `@GetMapping(headers = "mode=debug")`
- 미디어 타입 조건을 매핑할 수 있다. 
	- ex) `@PostMapping(consumes = "application/json")`
	- 안 맞으면 415 unsupported media type 반환
	- ex) `@PostMapping(produces = "text/html")`
	- 안 맞으면 406 Not Acceptable 반환
- 