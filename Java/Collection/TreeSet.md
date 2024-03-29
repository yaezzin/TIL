# TreeSet

TreeSet은 Set 인터페이스를 구현한 클래스로써 **중복해서 저장할 수 없고 저장 순서가 유지되지 않는다.**
* TreeSet은 ```이진 탐색 트리(BinarySearchTree)``` 구조로 이루어져 있다.
* TreeSet의 경우 이진 탐색 트리의 성능을 향상시킨 ```레드-블랙 트리```로 구현되어있다.


## 이진 탐색 트리
```java
class TreeNode {
    TreeNode left;  // 왼쪽 자식 노드
    Object element; // 객체를 저장하기 위한 참조변수
    TreeNode rightl // 오른쪽 자식 노드
}
```

이진 탐색 트리는 정렬, 검색, 범위검색에 높은 성능을 보이는 자료구조이다.
* 이진 탐색 트리는 링크드리스트처럼 여러 개의 노드가 서로 연결된 구조로, 각 노드에 최대 2개의 노드를 연결할 수 있다.
* 이진 탐색 트리는 부모노드의 ```왼쪽```에는 부모노드의 값보다 **작은 값**의 자식노드를, ```오른쪽```에는 **큰 값**의 자식노드를 저장한다

## 값이 저장되는 과정
<img width="400" alt="스크린샷 2023-01-26 오후 4 14 43" src="https://user-images.githubusercontent.com/97823928/214777687-087f91bd-830c-4df9-9990-d52e968123db.png">

7,4,9,2,5를 차례대로 TreeSet에 저장한다면 위와 같은 과정을 거치게 된다.
* 왼쪽 마지막 값에서부터 오른쪽 값까지 값을 ```왼쪽노드 -> 부모노드 -> 오른쪽노드```순으로 읽어오면 ```오름차순```으로 정렬된 순서이다. 
* 이처럼 정렬된 상태를 유지하기 떄문에 단일 값 검색과 범위검색이 매우 빠르다
* 반면 트리는 데이터를 순차적으로 저장하는 것이 아닌 저장위치를 찾아서 저장해야하고, 삭제하는 경우 트리의 일부를 재구성해야 하므로 링크드리스트보다 데이터의 추가/삭제 시간은 더 걸린다.
