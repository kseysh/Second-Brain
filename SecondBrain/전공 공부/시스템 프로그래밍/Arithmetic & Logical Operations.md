# `leaq` Source, Dest
leaq = Load Effective Address 
movq (  ), %~ 는 주소에 가서 주소에 있는 값을 register에 넣는다면, 
leaq (  ), %~ 는 주소만 계산해서 주소를 register에 넣는다. (address만 load하는 것)
- Source는 메모리, Dest는 register가 오게 된다.

### 사용 하는 곳
ex ) p = &x\[i];
주소 값을 어딘가에 넣을 때