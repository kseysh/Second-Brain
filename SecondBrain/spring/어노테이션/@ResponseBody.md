1. `@ResponseBody` 어노테이션:
    - `@ResponseBody` 어노테이션은 컨트롤러 메서드가 반환하는 데이터를 HTTP 응답의 본문(body)으로 사용하도록 지정한다.
    - 주로 JSON 같은 데이터를 클라이언트에 전달할 때 사용된다.
    - 이 어노테이션을 사용하면 스프링은 메서드가 반환한 데이터를 적절한 형식으로 직렬화하고 HTTP 응답의 본문으로 포함시킨다.
    - 주로 RESTful API를 개발할 때 사용되며, 메서드가 객체를 반환하면 해당 객체는 JSON으로 변환되어 클라이언트에게 전달된다.

예시:

```
@RestController public class MyController {     @GetMapping("/data")     
@ResponseBody     
public MyData getData() {         
	MyData data = new MyData("Hello, World!");         
	return data;     
	} 
}
```