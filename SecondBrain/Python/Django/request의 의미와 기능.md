
django에서는 view의 함수를 불러올 때 request를 매개변수로 넘겨준다. 이 때 request는 어떠한 역할을 할까?

request 매개변수는 request에 대한 데이터를 포함하는 HttpRequest 객체이다.

django가 request를 받게 되면 매칭되는 method를 찾아서 이를 불러오며 request를 보내준다. 

관련 django docs (HttpRequest objecets) : https://docs.djangoproject.com/en/4.2/ref/request-response/












