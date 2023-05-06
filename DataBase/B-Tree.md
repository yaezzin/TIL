## 인덱스 자료구조 

## 1. B+ Tree 인덱스 알고리즘

<img width="500" alt="스크린샷 2022-08-17 오후 9 38 18" src="https://user-images.githubusercontent.com/97823928/185127180-618c1123-61b6-46d3-87a9-b2717ab1df0d.png">

기존의 B- 트리는 어느 한 데이터의 검색은 효율적이나, 모든 데이터를 한 번에 순회하기 위해 모든 노드를 방문하는 비효율성을 가진다. 

반면 B+ Tree는 ```leaf node에만 데이터를 저장```하고 
```leaf 노드를 제외한 노드에는 자식 포인터를 저장```하며 leaf node끼리는 ```Linked List```로 연결되어있다.

### 장점
> 검색 속도의 향상
* leaf 노드 외에는 데이터를 저장하지 않으므로 메모리 확보가 가능해진다.
* 그러므로 하나의 노드에 더 많은 포인터를 가질 수 있기 때문에 트리의 높이가 더 낮아지므로 검색 속도가 높아질 수 있다.

> Full scan에 선형시간 소요
* B+ 트리의 경우 leaf 노드에만 데이터가 저장되어있고, 서로 linked list로 연결되어있기 때문에 선형시간이 소모된다. 
* B- 트리의 경우 모든 노드를 확인해야 하는 단점이 존재

### 단점
* B- 트리의 경우 특정 키를 루트노드에서 찾을 수 있으나 B+ Tree는 반드시 특정키에 접근하려면 leaf node까지 가야한다.

## 2. 해시 테이블 (Hash table)

> Key와 value를 한 쌍으로 데이터를 저장하는 자료구조

* 해시 테이블을 사용하면 인덱스는 ```(Key, Value) = (컬럼의 값, 데이터의 위치)```로 구현한다.
* 컬럼의 값으로 해시 값을 계산해서 인덱싱하므로 O(1)의 시간만에 데이터를 탐색할 수 있다.
* 하지만 값을 변형하여 인덱싱하므로, 특정 문자로 시작하는 값을 검색하는 것처럼 일부만으로 검색하고자할 때는 해시 인덱스를 사용할 수 없다.

> 시간복잡도가 O(1)임에도 불구하고 해시테이블은 실제로 인덱스에서 잘 사용하지 않는다.
* 해시 테이블은 등호 ( = ) 연산에 최적화되어있기 때문이다.
* SELECT 질의의 조건에는 부등호 연산이 자주 사용되는데, 해시 테이블 내의 데이터들은 정렬되어있지 않으므로 특정 기준보다 크거나 작은 값을 빠른 시간 내에 찾을 수 없다.

