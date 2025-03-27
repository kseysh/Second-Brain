## Stream Cipher
- one-Time pad의 일반화라고 볼 수 있다.
- digital data stream을 한 번에 1비트 또는  1byte씩 암호화 한다.
- key stream이 무작위라면, unbreakable하다.
- 전송할 데이터 트래픽이 크다면, 저장의 문제로 인해 해결하기 어렵다
	- 따라서 key와 key를 seed로 하는 key generator를 이용하여 plaintext를 암호화하는 방식을 사용한다.
	- 사용자는 generating key만 공유하면 되고, 각자 

ex) Autokeyed Vigenère cipher, Vernam cipher
