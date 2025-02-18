![[Pasted image 20250219000251.png|400]]
- @ResponseBody를 사용하면
	- ViewResolver대신 HttpMessageConverter가 작동한다.
	- 기본 문자 처리 - StringHttpMessageConverter
	- 기본 객체처리 - MappingJackson2HttpMessageConverter
	- byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음
- 응답의 경우 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해서 HttpMessageConverter가 선택된다.

## HTTP 메시지 컨버터 인터페이스
```java
public interface HttpMessageConverter<T> {

	// 메시지 컨버터가 해당 클래스, 미디어 타입을 지원하는지 체크 ex) byte[], String, class
	boolean canRead(Class<?> clazz, @Nullable MediaType mediaType); 
	boolean canWrite(Class<?> clazz, @Nullable MediaType mediaType);
	
	List<MediaType> getSupportedMediaTypes();

	// 메시지 컨버터를 이용해 메시지를 읽고 씀
	T read(Class<? extends T> clazz, HttpInputMessage inputMessage) throws IOException, HttpMessageNotReadableException;
	void write(T t, @Nullable MediaType contentType, HttpOutputMessage outputMessage) throws IOException, HttpMessageNotWritableException;
}
```

## HTTP 메시지 컨버터 사용 위치
@RequestMapping을 처리하는 핸들러 어댑터인 RequestMappingHandlerAdapter에서 사용해준다.
![[Pasted image 20250219001423.png|400]]
### ArgumentResolver
@RequestParam, @ModelAttribute, @RequestBody, HttpEntity 등 여러 파라미터를 유연하게 처리할 수 있도록 도와주는 객체
