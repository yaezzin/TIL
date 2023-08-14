## 큐 Queue

처음 들어간 데이터가 먼저 나오는 구조 FIFO
* 데이터의 ```추가``` & ```삭제``` & ```맨 앞의 데이터 조회``` → ```O(1)```

## 구현

```python
from collections import deque

deque = deque()

deque.append(1)
deque.popleft()
```


#### 스택과 달리 큐를 list로 이용하지 않는 이유

* list.pop(0)을 이용하면 리스트를 큐처럼 사용할 수 있음
* 하지만 pop()의 시간복잡도는 O(1)인 반면, **pop(0)의 시간복잡도는 O(N)**
