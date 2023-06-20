# 덱 Deque

<img width="500" alt="스크린샷 2022-07-08 오후 8 37 21" src="https://user-images.githubusercontent.com/97823928/177985030-ceb7a8dc-8ff1-4068-96e8-a616ab6b9113.png">

* 큐의 경우 한쪽에서 삽입이 이루어지고 다른 한쪽에서 삭제를 하는 선입선출(FIFO) 구조를 가진다.
* 반면 양쪽에서 삽입, 삭제가 가능한 **양방향** 큐를 ```덱```이라고 한다
* 덱은 큐의 연산을 포함하고 있다!

## 명령어

|명령어|설명|시간복잡도|
|------|---|:----------:|
|push_front(v)|v값을 맨 앞에 넣음|```O(1)```|
|push_back(v)|v값을 맨 뒤에 넣음|```O(1)```|
|pop_front()|맨 앞의 값을 뽑음|```O(1)```|
|pop_front()|맨 뒤의 값을 뽑음|```O(1)```|
|front()|맨 앞의 값을 조회|```O(1)```|
|back()|맨 뒤의 값을 조회|```O(1)```|

* 덱의 경우 push, pop 연산이 리스트보다 훨씬 속도가 빠르다!

## 사용법

```python
from collections import deque

dq = deque()

dq.appendleft(1)
dq.append(2)

dq.popleft()
dq.pop()
```

```python
dq = deque([1, 2, 3, 4, 5])
dq.rotate(1) # deque([5, 4, 3, 2, 1]) -> 오른쪽으로 회전
dq.rotate(-1) # deque([1, 2, 3, 4, 5]) -> 왼쪽으로 다시 회전
```


## 참고자료
* https://www.programiz.com/dsa/deque
