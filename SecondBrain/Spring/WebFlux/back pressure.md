구독자가 Producer의 많은 데이터를 다 처리하려고 하면 부하가 발생할 수 있다.
따라서 Reactive Programming에서는 과부하를 방지하기 위해 back pressure를 도입해  다운스트림이 업스트림에 더 적은 양의 데이터를 보내라고 지시할 수 있다. 

ex)request() 함수를 사용하여 업스트림에 한 번에 두개의 요소만 전송하도록 하는 예제
```java
Flux.just(1, 2, 3, 4)
  .log()
  .subscribe(new Subscriber<Integer>() {
    private Subscription s;
    int onNextAmount;

    @Override
    public void onSubscribe(Subscription s) {
        this.s = s;
        s.request(2);
    }

    @Override
    public void onNext(Integer integer) {
        elements.add(integer);
        onNextAmount++;
        if (onNextAmount % 2 == 0) {
            s.request(2);
        }
    }

    @Override
    public void onError(Throwable t) {}

    @Override
    public void onComplete() {}
});
```