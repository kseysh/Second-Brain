# Stream.toList()의 장점
java10에서 리턴되는 List가 수정이 가능한 Collectors.toList()와 다르게 수정 불가능한 List로 반환되도록 `toUnmodifiableList()`가 등장하였다.
하지만 `toUnmodifiableList()`는 이름이 장황하여 이를 보완하기 위해 Stream.toList()가 나왔다.

### 수정이 불가능한 List에 수정을 하려한다면?
`UnsupportedOperationException.class` 에러를 던진다.


## Null 허용 여부
.collect(Collectors.toList()), .collect(Collectors.toUn modifiableList()), Stream.of.toList() 는 수정여부 이외에 Null 허용 여부에 대해 차이가 있다.
- Collectors.toList()
	- Nullable
- Collectors.toUnmodi