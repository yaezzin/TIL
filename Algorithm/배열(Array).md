## 배열

> 동일한 유형의 데이터를 순차적으로 저장

## 배열의 특징

* ```메모리에 연속적으로 배치```되어 인덱스로 접근 가능
* 추가적으로 소모되는 메모리의 양(= 오버헤드)가 거의 없음
* Cache hit rate가 높음
* 메모리상에 연속한 구간을 잡아야 해서 할당에 제약이 걸림

## 1. 확인/변경

* 배열 내에서 K번째 인덱스에 해당하는 값을 찾아내는 연산 → ```O(1)```
* 배열의 첫 번째 원소의 주소를 알고 있으니 i번째 원소에 접근하기 위해서는 **첫 번째 원소의 주소 + (자료형 크기)*i**한 주소에 접근

## 3. 삽입 

* 배열의 끝에 원소를 추가 → ```O(1)```
* 임의의 위치에 원소를 추가  → ```O(N)```
  * K번째에 원소를 추가하게 되면 그 뒤의 원소들을 모두 한 칸씩 밀어야 함 

## 4. 삭제

* 배열의 끝에 원소를 삭제 → ```O(1)```
* 임의의 위치에 원소를 삭제  → ```O(N)```
  * K번째에 원소를 삭제하게 되면 앞의 원소들을 모두 한 칸씩 당겨야 함

## 5. 함수

list = [1, 2, 3, 4]

* list.```insert(index, value)```
* list.```append(value)```
* ```del``` list[index]
* list.```remove(value)```
* ```len```(list)
