# 스택

가장 먼저 넣은 데이터가 가장 마지막에 빠져나오는 데이터 구조 - ```Last In First Out(LIFO)``` 

```python
class Stack:
    def __init__(self):
        self.items = []

    def is_empty(self):
        return len(self.items) == 0

    def push(self, item):
        self.items.append(item)

    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        else:
            print("Stack is empty.")

    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        else:
            print("Stack is empty.")
```
* ```list.append()``` : 맨 뒤에 요소를 추가
* ```list.pop()``` : 맨 뒤의 요소를 제거
* ```peek``` : 맨 위에 있는 요소를 가져옴

## 사용법

```python
stack = Stack()
print(stack.is_empty())  # True

stack.push(10)
stack.push(20)
stack.push(30)
print(stack.size())  # 3
print(stack.peek())  # 30

print(stack.pop())  # 30
print(stack.pop())  # 20
```
