Django를 공부하던 중 Foreign key에 대한 이해가 필요하여 작성합니다.

`Book`과 `Author`의 관계에서 `Author` 하나당  여러개의 `Book` 을 가지고 있으므로 이는 일 대 다 관계라고 부를 수 있습니다.

참조(Forward Reference):

- 참조(Forward reference)는 Foreign key를 사용하여 다른 모델을 참조하는 방향입니다.
- 예를 들어, `Book` 모델이 `Author` 모델을 참조한다고 가정해보겠습니다. 이때 `Book` 모델의 `author` 필드는 `Author` 모델의 Primary key를 참조합니다.
- 참조를 통해 `Book` 객체에서 `author` 필드를 사용하여 해당 책의 저자를 참조할 수 있습니다. 예를 들어, `book.author`를 통해 책의 저자를 얻을 수 있습니다.

역참조(Reverse Reference):
- 역참조(Reverse reference)는 참조된 모델에서 다시 참조한 모델로의 역방향 관계입니다.
- 위의 예에서 `Author` 모델에서 `Book` 모델로의 역참조를 생각해보겠습니다. `Book` 모델이 `Author` 모델을 참조하고 있으므로, `Author` 객체는 역참조를 통해 해당 저자와 관련된 모든 책을 가져올 수 있습니다.
- 역참조를 통해 `Author` 객체에서 `book_set` 속성을 사용하여 해당 저자와 연결된 모든 책을 가져올 수 있습니다. 예를 들어, `author.book_set.all()`을 사용하여 저자와 연결된 모든 책을 얻을 수 있습니다.
> 쉽게 이해하려면 일대다 관계이므로 '일'에서 '다'를 참조하게 되면 참조되는 수가 많을 것이므로 이름 뒤에 \_set을 사용하여 참조하도록 하는 것입니다.
- `related_name` 옵션을 사용하여 역참조에 사용할 이름을 지정할 수도 있습니다. 이를 통해 역참조를 더 명확하게 할 수 있습니다.

`models.ForeignKey`는 1대다 관계에서는 참조를 하는 '1'쪽에서 사용하여 '다'쪽을 참조하게 된다. 


