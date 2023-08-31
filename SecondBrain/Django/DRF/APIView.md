## CreateAPIView

create를 위해 사용된다.
post method를 사용한다.

## ListAPIView

model instance의 콜렉션의 read를 위해 사용된다.
get method를 사용한다.

## RetrieveAPIView

single model instance의 read를 위해 사용된다.
get method를 사용한다.

## DestroyAPIView

single model instance의 delete를 위해 사용된다.
delete method를 사용한다.

## UpdateAPIView

single model instance의 update를 위해 사용한다.
put 또는 patch method를 사용한다.


## 이외의 둘의 기능을 섞은 APIView

> 이것을 사용하는 이유?
> => restful하게 코드를 작성하려면 url이 같아야하기 때문!
> [[RESTful 한 API란]]

### ListCreateAPIView

### RetrieveUpdateAPIView

### RetrieveDestroyAPIView

### RetrieveUpdateAPIView

### RetrieveUpdateDestroyAPIView

## 기능을 섞은 APIView를 사용할 때

같은 url로 요청을 보내더라도, 요청이 get이냐, put/patch냐 delete냐에 따라 기능 수행이 달라진다!




