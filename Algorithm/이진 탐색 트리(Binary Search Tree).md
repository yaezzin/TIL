## 이진 탐색 트리

<img width="351" alt="스크린샷 2023-08-17 오후 2 36 16" src="https://github.com/yaezzin/TIL/assets/97823928/3fcba243-6fe2-4ca1-91ae-52bcc468157a">

* 왼쪽 노드는 해당 노드보다 작은 값, 오른쪽 노드는 해당 노드보다 큰 값을 가지고 있음
* 중복된 값은 들어올 수 없음

* 이진 탐색트리는 각 노드에서 왼쪽 또는 오른쪽으로 이동하며 탐색하기 때문에 탐색 경로가 **트리의 높이에 비례** → ```O(log n)```
  ```
  N = 2^(h+1) -1
  N + 1 = 2^(h+1)
  log(N + 1)  = h + 1
  h = log(N+1) - 1
  즉, O(log N)
  ```
* 하지만 이진 탐색 트리가 한 쪽으로 기울어져 있는 경우에는 연결 리스트와 유사한 형태가 되어 최악의 시간 복잡도가 ```O(n)```이 될 수 있음

## 구현

### Node

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```

### Tree

```python
class BinaryTree:
    def __init__(self, root = None):
        self.root = root
```

### 삽입

```python
def insert(self, value):
    new_node = Node(value)

    # root가 존재하지 않으면 현재 노드를 루트로 함
    if self.root is None:
        self.root = new_node

    current_node = self.root
    while True:
        # 값이 더 작으면 왼쪽으로
        if value < current_node.value:
            if current_node.left is None:
                current_node.left = new_node
                break
            else:
                current_node = current_node.left # 왼쪽 자식 노드로 이동하여 계속 탐색

        else:
            if current_node.right is None:
                current_node.right = new_node
                break
            else:
                current_node = current_node.right
```

### 조회

```python
def search(self, value):
    current_node = self.root

    while current_node is not None:
        if current_node.value == value:
            return True
  
        elif current_node.value > value:
            current_node = current_node.left
  
        else:
            current_node = current_node.right
    reutrn False
```

### 삭제

<img width="128" alt="스크린샷 2023-08-17 오후 1 39 39" src="https://github.com/yaezzin/TIL/assets/97823928/8fb36241-a491-4bcf-b4c1-412c5883e4d1">

```python
def delete(self, value):
    current_node = self.root
    parent_node = None

    # 삭제할 노드를 찾을 때까지 탐색
    while current_node and current_node.value != value:
        if current_node.value > value:
            current_node = current_node.left
        else:
            current_node = current_node.right

    # 만약 삭제할 노드가 없으면 종료
    if current_node is None:
        return

    # CASE 1 : 찾은 노드의 자식 노드가 아예 없는 경우
    if current_node.left is None and current_node.right is None:
        if parent_node is None:
            self.root = None
        if parent_node.left == current_node:
            parent_node.left = None
        else:
            parent_node.right = None

    # CASE 2 : 찾은 노드의 자식 노드가 1개인 경우
    if (current_node.left is None) != (current_node.right is None):
        if value < parent_node.value:
            if current_node.left:
                parent_node.left = current_node.left
            else:
                parent_node.left = current_node.right
        else:
            if current_node.left:
                parent_node.right = current_node.left
            else:
                parent_node.right = current_node.rigth

    # CASE 3 : 찾은 노드의 자식 노드가 2개인 경우
    if current_node.left and current_node.right:
        successor = current_node.right
        successort_parent = current_node

        while successor.left:
            successor_parent = successor
            successor = successor.left

        if successor_parent != current_node:
            successor_parent.left = successor.right
        else:
            successort_parent.right = successor.right

        current_node.value = successor.value # 삭제할 노드의 값을 후속자 노드의 값으로 갱신

```
