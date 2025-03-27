## Stream Cipher
![[Pasted image 20250327135302.png|300]]
- one-Time pad의 일반화라고 볼 수 있다.
- digital data stream을 한 번에 1비트 또는  1byte씩 암호화 한다.
- key stream이 무작위라면, unbreakable하다.
- 전송할 데이터 트래픽이 크다면, 저장의 문제로 인해 해결하기 어렵다
	- 따라서 key와 key를 seed로 하는 key generator를 이용하여 plaintext를 암호화하는 방식을 사용한다.
	- 사용자는 generating key만 공유하면 되고, 각자 key stream을 생성할 수 있다 (대칭 암호 방식)
ex) Autokeyed Vigenère cipher, Vernam cipher
## Block Cipher
![[Pasted image 20250327135317.png|300]]
- playfair의 숫자 단위 크기를 늘리는 느낌이다.
- 평문 블록을 전체 단위로 처리하여 동일한 길이의 암호문 블록을 생성한다.
- 일반적으로 64~128 비트 블록 크기가 상용
- 대칭 암호화 키를 공유한다.
- 대부분의 네트워크 기반 대칭 암호화 응용 프로그램은 블록 암호를 사용한다.
#### 문제
4 bit에서 Decryption table은 2<sup>4</sup>개의 element를 가지는데, 128bit라면 Descryption table이 2<sup>128</sup>개가 되어버려 너무 큰 값이 되어 저장하기 어려워진다.
따라서 Decrypting table이 저장하기에 너무 커서 Playfair cipher처럼 알고리즘을 이용하도록 한다.
## Feistel Cipher
치환과 순열을 번갈아 사용하는 암호 기법
혼동과 확산 기능을 번갈아 사용하는 product cipher 개념의 실용적 적용
현재 사용되는 많은 주요 대칭 블록 암호들이 이 구조를 기반으로 한다.
#### 치환
각 평문의 요소 또는 요소 그룹이 고유하게 대응하는 암호문 요소 또는 요소 그룹으로 대체된다.
#### 순열
요소가 추가되거나 삭제되거나 대체되지 않으며, 단순히 요소의 순서만 변경된다.
### Diffusion
각 plaintext의 숫자가 여러 개의 암호문 숫자에 영향을 미치도록 한다.
### Confusion
암호문의 통계적 특성과 암호화 키의 값 간의 관계를 최대한 복잡하게 만든다.
암호문의 통계 정보를 얻더라도, 해당 정보를 바탕으로 키를 유추하기 어렵도록 설계한다.
![[Pasted image 20250327140948.png|200]]
이렇게 16번 다시 돌리면 Decryption이 됨
