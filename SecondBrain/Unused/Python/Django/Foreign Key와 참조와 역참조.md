Django를 공부하던 중 Foreign key에 대한 이해가 필요하여 작성합니다.

`Book`과 `Author`의 관계에서 `Author` 하나당  여러개의 `Book` 을 가지고 있으므로 이는 일 대 다 관계라고 부를 수 있습니다.

[[참조]]

[[역참조]]

`models.ForeignKey`는 1대다 관계에서는 참조를 하는 '1'쪽에서 사용하여 '다'쪽을 참조하게 된다. 


