# Spring MVC 구조
![[Pasted image 20250218224243.png]]
# DispatcherServlet 요청 흐름
- 서블릿 호출시 `HttpServlet`이 제공하는 `service()` 호출
- `HttpServlet`을 상속한 `FrameworkServlet.service()`를 시작으로 여러 메서드가 호출되면서 `DispatcherServlet.doDispatch()` 호출
# DispatcherServlet.doDispatch() 동작 흐름
```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {

	HttpServletRequest processedRequest = request;
	HandlerExecutionChain mappedHandler = null;
	ModelAndView mv = null;
	
	// 1. 핸들러 조회
	mappedHandler = getHandler(processedRequest);
	if (mappedHandler == null) {
		noHandlerFound(processedRequest, response);
		return;
	}
	
	// 2. 핸들러 어댑터 조회 - 핸들러를 처리할 수 있는 어댑터
	HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
	
	// 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환
	mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
	
	processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
}

private void processDispatchResult(HttpServletRequest request, HttpServletResponse response, HandlerExecutionChain mappedHandler, ModelAndView mv, Exception exception) throws Exception {

	// 뷰 렌더링 호출
	render(mv, request, response);
}

protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {

	View view;
	String viewName = mv.getViewName();
	
	// 6. 뷰 리졸버를 통해서 뷰 찾기, 7. View 반환
	view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
	
	// 8. 뷰 렌더링
	view.render(mv.getModelInternal(), request, response);
}
```
- 핸들러 조회
- 핸들러 어댑터 조회
- 핸들러 어댑터 실행
- 핸들러 실행
- ModelAndView 반환
- viewResolver 호출
- view 반환
- 뷰 렌더링
