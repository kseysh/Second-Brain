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
따라서 저장하기에 