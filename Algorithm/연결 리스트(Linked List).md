## 연결 리스트

* N번째 원소를 확인/변경 → ```O(N)```
  * 배열과 달리 연속된 공간에 위치하지 않고, 5를 찾고 싶으면 3, 13, 72를 거쳐서 순차적으로 가야하므로!
* 임의의 위치에 원소를 추가/삭제 → ```O(1)```
  * 배열과 달리 원소들을 옮기는 작업이 필요 없고, 주소 변경만 해주면 됨

## 연결 리스트 종류

<img width="600" alt="스크린샷 2023-08-14 오전 10 44 28" src="https://github.com/yaezzin/TIL/assets/97823928/6e95984e-8e14-4fe1-8ca3-c9af3b731ce6">

* 단일 연결 리스트 : 각 원소가 자신의 다음 원소의 주소를 들고 있는 구조
* 이중 연결 리스트 : 이전 원소와 다음 원소의 주소를 둘 다 들고 있는 구조
* 원형 연결 리스트 : 끝이 처음과 연결되어 있는 구조 + 이중 연결 리스트의 특징을 가짐

## 배열 vs 연결 리스트

<img width="600" alt="스크린샷 2023-08-14 오전 10 43 02" src="https://github.com/yaezzin/TIL/assets/97823928/6b1fe8d2-d93d-48a9-8100-5982659e8357">

## 구현

```python
class Node:
    def __init__(self, data):
        self.data = data  #data는 값을 가리키는 변수(속성, attribute)
        self.next = None  #next는 다음 노드를 가리키는 변수
```

#### 1. 연결 

```python
head_node = Node(3)
head_node.next = Node(13)
head_node.next.next = Node(72)
```

#### 2. 맨 앞 노드에 삽입 : O(1)

```python
def addHead(self, val):
    new_node = Node(val)
    new_node.next = self.head # 새 노드의 다음 노드를 현재 헤드로 설정
    node.head = new_node # 헤드를 새 노드로 업데이트
```

#### 3. 맨 뒤 노드에 삽입 : O(N)

```python
def addBack(self, val):
    new_node = Node(vale)
    current_node = self.head # 연결리스트의 첫번째 노드부터 시작하여 순회
    while current_node.next:
        current_node = current_noede.next
    current_node.next = new_node # 새로운 노드를 마지막 노드의 다음에 연결
```

#### 4. 탐색 : O(N)

```python
def find(self, val):
    current_node = self.head
    while current_node is not None:
        if current_node.val == val:
            return current_node
        current_node = current_node.next
    raise RuntimeError('Node Not Found')
```

#### 5. 특정 노드의 뒤에 삽입 : O(1)

```python
def addAfter(self, node, val):
    new_node = Node(val)
    new_node.next = node.next
    node.next = new_node
```

#### 6. 특정 노드의 다음 노드 삭제 : O(1)

```python
def deleteAfter(self, prev_node):
    if prev_node.next is not None:
        prev_node.next = pre_node.next.next
```



 


