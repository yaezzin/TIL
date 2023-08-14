## 스택 Stack

<img width="200" alt="스크린샷 2023-08-14 오후 1 23 24" src="https://github.com/yaezzin/TIL/assets/97823928/ce616ecb-2013-4b86-9405-c60db77b145a">

가장 나중에 넣은 데이터를 가장 먼저 빼낼 수 있는 데이터 구조 ```LIFO```
* 원소의 ```추가``` & ```제거``` & ```상단 원소 확인``` → ```O(1)```
* 파이썬에는 이미 list로 구현되어 있음

## 구현 

```python
class Stack:
    def __init__(self):
        self.stack = []

    # 원소 삽입
    def push(self, item):
        self.stack.append(item)

    # 맨 위의 원소 삭제/반환
    def pop(self):
        if not self.isEmpty():
            return self.stack.pop()
        else:
            exit()

    # 맨 위의 원소를 반환
    def peek(self):
        return self.stack[-1]

    # 비어있는지 확인
    def isEmpty(self):
        return not self.stack
```
