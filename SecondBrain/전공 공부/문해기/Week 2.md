string fileString = "" // 입력으로 주어지는 파일 스트링
bool openTag = false // 현재 '<' 이후이고, '>' 이전인지
bool closeTag = false // 현재 '</' 이후이고, '>' 이전인지
// 항상 시작하기 전 사용할 변수의 이름과 타입을 정해두고 시작한다.
그래야 수도코드나 실제 코드를 짤 때 변수의 네이밍으로 헷갈리는 경우가 많이 줄어든다고 생각하기 때문이다.

파일 스트링의 길이만큼 반복한다 {
	char c  = fileString\[i];
	c가 '<'라면{
		뒤에 /가 있는지 확인하기 위해 i + 1이 파일스트링을 넘는다면 for문을 그대로 끝낸다.
		뒤에 /가 있다면 closeTag를 true로 놓는다.
		뒤에 /가 없다면 openTag를 true로 놓는다.
	} 
	c가 '>'라면{
		openTag가 true라면{
			stack에 저장했던 stringBuffer값을 넣는다.
			stringBuffer를 ""로 초기화한다.
			openTag를 false로 초기화한다.
		}
		closeTag가 true라면{
			stack에 저장했던 값을 지운다.
			closeTag값을 false로 초기화한다.
		}
	}
	c가 '>'와 
}

