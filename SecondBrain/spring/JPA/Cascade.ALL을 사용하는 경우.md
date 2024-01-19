```
@Transactional  
public Long order(Long memberId, Long itemId, int count) {  
    Member member = memberRepository.findOne(memberId);  
    Item item = itemRepository.findOne(itemId);  
  
    Delivery delivery = new Delivery();  
    delivery.setAddress(member.getAddress());  
  
    OrderItem orderItem = OrderItem.createOrderItem(item, item.getPrice(), count);  
  
    Order order = Order.createOrder(member, delivery, orderItem);  
  
    orderRepository.save(order);  
  
    return order.getId();  
}
```
- 위 예시에서 order를 order만 저장해도 되는 이유는 orderItems의 cascade = Cascade.ALL 옵션덕분에 하나만 저장해도 전부 영향이 가는 것이다
- order만 persist해도 orderItem이 전부 persist가 된다.  

## Cascade.ALL을 사용해도 괜찮은 수준
- persist는 order가 delivery를 관리하고, order가 orderitem을 관리하는데,  이런 정도의 수준에서만 사용하는 것이 좋다.  
- OrderItem과 Delivery는 Order이외의 곳에서는 사용하지 않는데, (Order만 참조해서 사용한다.)  
> 한 엔티티하고만 관계가 있는 엔티티에 사용하는 것이 좋다.  
- Cascade를 막 사용하게 되면 다른 관계를 맺는 엔티티인데 갑자기 예상치 못하게 지워지면 안되기 때문이다.  

이 개념이 잘 이해가 안된다면 차라리 안 사용하는 것이 현명 ! 
Cascade.ALL을 사용하지 않다가 리팩토링 과정에서 라이프사이클에서 persist할 때 함께 persist하거나,  
함께 delete할 때 Cascade.ALL로 바꾸는 것도 좋은 전략이다.  