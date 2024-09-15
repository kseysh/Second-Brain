

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
	c가 '>'와 '<'가 아니라면{
		openTag가 true라면{
			stringBuffer에 값을 추가한다.
		}
	}
}

스택에 남아있는 값들을 하나씩 pop하면서 형식에 맞게 출력한다.

