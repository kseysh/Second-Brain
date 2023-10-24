|Method|멱등성(Idempotent)|Cacheability|Safe|
|---|---|---|---|
|GET|O|O|O|
|POST|X|△|X|
|PUT|O|X|X|
|PATCH|X|△|X|
|DELETE|O|X|X|

(△ : 가능하지만 실제로 구현이 쉽지 않음)

[[멱등성]]
[[Cacheability]]
[[Safe]]