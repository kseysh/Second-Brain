SOPT에서 드디어 두 번의 협업을 경험하게 되었습니다.
서로 다른 서버 개발자 두 분에게 많이 배울 수 있는 시간이라 좋았던 것 같아요.
이렇게 협업을 경험해보며 아직 두 분의 서버 개발자 분들과만 협업을 해보았는데도 개발자마다 선호하는 방식이 다를 수 있구나라는 것을 느꼈습니다.

그 중 방식이 다르다는 것을 느꼈던 응답 반환에 관해서 이야기를 해보려고 합니다.

첫 번째 합동 세미나 때는 `ApiResponse`를 이용해 api를 전송하는 방식을 택했습니다!
## ApiResponse 객체
```java
@Getter  
@JsonPropertyOrder({"code", "message", "data"})  
@RequiredArgsConstructor(access = AccessLevel.PRIVATE)  
@AllArgsConstructor(access = AccessLevel.PRIVATE)  
public class ApiResponse<T> {  
  
    private final int code;  
    private final String message;  
  
    @JsonInclude(JsonInclude.Include.NON_NULL)  
    private T data;  
  
    public static ApiResponse<?> success(SuccessType successType) {  
        return new ApiResponse<>(successType.getHttpStatusCode(), successType.getMessage());  
    }  
  
    public static <T> ApiResponse<T> success(SuccessType successType, T data) {  
        return new ApiResponse<T>(successType.getHttpStatusCode(), successType.getMessage(), data);  
    }  
  
    public static ApiResponse<?> error(ErrorType errorType) {  
        return new ApiResponse<>(errorType.getHttpStatusCode(), errorType.getMessage());  
    }  
  
    public static ApiResponse<?> error(ErrorType errorType, String message) {  
        return new ApiResponse<>(errorType.getHttpStatusCode(), message);  
    }  
  
    public static <T> ApiResponse<T> error(ErrorType errorType, String message, T data) {  
        return new ApiResponse<>(errorType.getHttpStatusCode(), message, data);  
    }  
  
    public static <T> ApiResponse<Exception> error(ErrorType errorType, Exception e) {  
        return new ApiResponse<>(errorType.getHttpStatusCode(), errorType.getMessage(), e);  
    }  
  
    public static <T> ApiResponse<T> error(ErrorType errorType, T data) {  
        return new ApiResponse<>(errorType.getHttpStatusCode(), errorType.getMessage(), data);  
    }  
}
```

## ApiResponse를 이용해 구현한 Controller
위의 `ApiResponse`를 이용하여 아래와 같이 `Controller`에서 응답을 해주도록 하였습니다.
```java
private final OrderService orderService;  
  
@GetMapping  
public ApiResponse<List<OrderListResponse>> orderList() {  
    return ApiResponse.success(GET_ORDER_LIST_SUCCESS, orderService.findOrderList());  
}
```
spring으로 하는 첫 개발이었음에도 불구하고 직관적으로 확인할 수 있었던 코드였던 것 같아요.
저도 첫 개발이었지만 쉽게 습득하고 사용하기에 좋은 구조였습니다.

두 번째 SOPT의 해커톤, 일명 솝커톤에서의 응답 반환 방식은 약간 달랐습니다. `ApiResponse`를 사용하는 것 자체는 동일하지만, `ApiResponse`를 `ResponseEntity`로 감싸서 응답해준다는 것이 다른 점이었어요.

## ResponseEntity란?
HttpHeader와 HttpBody를 포함하는 클래스라 HTTP header, body, status를 함께 반환할 수 있게 해주는 클래스이다. 

## ResponseEntity를 이용해 구현한 Controller
```java
private final ProgramService programService;  
  
@GetMapping("/register")  
public ResponseEntity<ApiResponse<List<ProgramListResponse>>> programListViewByStatus() {  
    List<ProgramListResponse> programListByStatus = programService.getStatusDoneProgramList();  
    return ResponseEntity  
            .status(ProgramSuccess.PROGRAM_LIST_VIEW_SUCCESS.getHttpStatus())  
            .body(  
                    ApiResponse.success(ProgramSuccess.PROGRAM_LIST_VIEW_SUCCESS, programListByStatus)  
            );  
}
```
`ApiResponse`로만 하는 응답과는 많이 차이가 나죠..? 처음 이 구조를 보고서는 조금 놀라기도 했습니다. 뭔가 어렵고 `ApiResponse`로만 하는 응답보다 가독성이 떨어질 수 있을거라 생각이 들어서요.
그래서 ResponseEntity로 묶어서 응답을 하는 이유에 대해서 여쭤보게 되었습니다. 
## ResponseEntity를 사용하는 이유
`ApiResponse`만을 반환하게 되면 HTTP 응답 body에 포함되는 상태 코드로만 반영이 되지만, `ResponseEntity`를 이용해서 `ApiResponse`를 한 번 더 감싸고, `ResponseEntity`의 상태 코드들 설정하고 응답을 보내게 되면 `ApiRsponse`의 `HttpStatus` 외에도 실제 응답의 상태 코드를 설정해줄 수 있다고 합니다. 

이 방식을 사용하면 `ApiResponse`의 `HttpStatus`와 `ResponseEntity`의 `HttpStatus`를 동기화시킬 수 있는 큰 장점이 있습니다. 
하지만 단점이라고 한다면 역시 `ApiResponse`와 `ResponseEntity` 사이에 한 번의 추가적인 레이어가 존재한다는 점이겠죠. 
이를 통해 `ApiResponse`의 상태 코드와 `ResponseEntity`의 상태 코드를 동일하게 설정할 수 있지만, 중첩된 형태가 복잡해지고 코드 가독성이 떨어질 수 있습니다.

## 저는 어떤 응답과 맞냐면요..
저도 두 개발자 분들의 코드를 확인했을 때의 저에게 맞는 방법은 개발자가 좀 더 고생하더라도 `ResponseEntity`를 사용하는 방식을 택해보려고 합니다. 

좋은 글이 있길래 가져와봤어요
https://dev-splin.github.io/spring/Spring-ResponseEntity/
이 글에서는 `Restful API`를 만들었던 이유에 대해서 설명하면서 ResponseEntity를 사용해야 하는 이유에 대해서 설명하고 있는데요.
> Restful API를 만드는 가장 큰 이유는 Client Side를 정형화된 플랫폼이 아닌 모바일, PC, 어플리케이션 등 플랫폼에 제약을 두지 않는 것을 목표로 했기 때문이다.

이에 따라 개발자들은 Client Side를 고려하지 않고 Client에서 바로 객체로 치환가능한 형태의 데이터 통신을 지향하며 Client와 Server의 역할을 분리하였다고 합니다.

이 때 필요해진 것이 HTTP 표준 규약이고, 이 HTTP 표준 규약을 지켜야 클라이언트와의 통신이 원활해질 수 있다고 합니다. **표준을 지키지 않게 되었을 때 발생하는 가장 큰 이슈는 HTTP Status 코드를 제대로 응답하지 않게 되는 것**이고 이런 경우 클라이언트에서는 별도의 방어코드를 넣는 수고가 발생하며, 이런 수고들이 모여 프로젝트의 속도를 저하시키는 요소가 되는 것이므로 표준을 준수하는 것이 중요하다는 말씀을 해주시더라구요.

이런 이유로 저도 `Controller`에서 응답을 반환할 때 `ResponseEntity`객체를 이용해 규약에 맞는 `HTTP Response`를 반환하려는 노력을 하려고 합니다.

물론 이건 제 생각이고 `ApiResponse`의 가독성이 좋다는 장점은 큰 장점이라고 생각합니다. 따라서 상황에 맞게 잘 사용할 수 있는 것이 개발자의 역량이라고 생각해요. 해커톤이나 이번에 했던 합동세미나처럼 크지 않고 작은 프로젝트에서는 `ApiResponse`를 사용하는 것이 빠르고 직관적인 개발을 할 수 있어 더 좋은 코드라고 생각되기도 합니다.

다만 어떤 응답을 사용하던 원래 이렇게 해왔어~ 라고 해서 개발하는 것이 아닌 근거있는 선택을 하는 것이 개발자의 역량이 아닐까 싶습니다!