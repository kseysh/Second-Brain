API서버 구성요소 중 Response를 구성하는 여러가지 방법 중에서 직접 Heaer와 body, ErrorCode를 써서 구성하는 방법이다.

기본적으로 ResponseEntity.status(HttpStatus.OK)를 사용해서 응답을 할 수도 있지만, 프로젝트를 진행할 때에 팀원끼리 약속된 응답형식과 코드를 통해 사용을 할 수도 있다.

## 예시
### ApiHeader
```java
public class ApiHeader 
{ 
	private int resultCode; // 성공 200, 실패 999, 600 
	private String codeName; // success, fail, NOT_FOUND_USER

	public ApiHeader(int resultCode, String codeName) {
		this.resultCode = resultCode; 
		this.codeName = codeName;
	} 
	public int getResultCode() { 
		return resultCode; 
	} 
	public String getCodeName() { 
		return codeName; 
	} 
}
```
### ApiBody
```java
public class ApiBody <T>{ 
	private T data; 
	private T msg; 
	public ApiBody(T data, T msg){ 
		this.data = data; 
		this.msg = msg; 
	} 

	public T getData(){ 
		return data; 
	} 

	public T getMsg(){ 
		return msg; 
	} 
}
```
### ApiResponse
```java
public class ApiResponse<T> { 
	private ApiHeader header; 
	private ApiBody body; 
	private static int SUCCESS = 200; 
	public ApiResponse(ApiHeader header, ApiBody body) { 
		this.header = header; 
		this.body = body; 
	} 
	public ApiResponse(ApiHeader header) { 
		this.header = header; 
	} 
	public static <T> ApiResponse<T> OK(T data) { 
		return new ApiResponse<T>(new ApiHeader(SUCCESS, "SUCCESS"), new ApiBody(data,null)); 
	} 
	public static <T> ApiResponse<T> fail(ErrorCode errorCode) { 
			return new ApiResponse(new ApiHeader(errorCode.getCode(), errorCode.name()), new ApiBody(null, errorCode.getMessage())); } 
	}
```

### ErrorCode
```
public enum ErrorCode { 
	FAIL(999, "실패"), 
	NOT_SUPPORTED_METHOD(998, "NOT_SUPPORTED_METHOD"), 
	NOT_FOUND_USER(100, "NOT_FOUND_USER"), 
	NOT_FOUND_TOKEN(101, "NOT_FOUND_TOKEN"), 
	MALFORMED_ERROR(102, "MALFORMED_ERROR"); 

	private int code; private String message; 

	ErrorCode(int code, String message) { 
		this.code = code; 
		this.message = message; 
	} 

	public int getCode() { 
		return code; 
	} 
	
	public String getMessage() { 
		return message; 
	} 
}
```

### RequestMapping
```
@RequestMapping(method = RequestMethod.GET, value="/getUser") 
	public @ResponseBody ApiResponse<UserDTO> getUser(HttpServletRequest request, String user_id){ 
	if(user_id == null){ 
		return ApiResponse.fail(ErrorCode.FAIL); 
	} 
	return ApiResponse.OK(userService.getUser(user_id)); 
}
```