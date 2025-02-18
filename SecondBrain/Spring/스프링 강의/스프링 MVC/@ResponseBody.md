@ResponseStatus(HttpStatus.OK)와 함께 사용하면
ResponseEntity.ok()처럼 작동한다.
ResponseEntity.ok()가 더 깔끔한 듯

@RestController 애노테이션 안에 @ResponseBody가 적용되어 있어 사용하지 않아도 된다.