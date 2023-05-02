## HashSet

```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable {}
```
[```HashSet```](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/HashSet.html)은 Set 인터페이스를 구현한 가장 대표적인 컬렉션이다.
* 중복된 요소를 저장하지 않으며, null을 허용한다.
* 순서를 유지하지 않는다. (순서를 유지하고 싶으면 LinkedHashSet을 사용해야 한다.)
* 내부적으로 HashMap을 이용해서 만들어졌다.

## 생성 방법

|생성자 |설명|
|----|----|
|HashSet()| 객체 생성|
|HashSet(Collection c)|주어진 컬렉션을 포함하는 객체 생성|
|HashSet(int initialCapacity)|주어진 값을 초기용량으로 하는 객체 생성|
|HashSet(int initialCapacity, float loadFactor)| 초기용량과 로드팩터를 지정하는 생성자|

* capacity(용량)는 저장용량을 의미하고, loadFactor(로드팩터)는 데이터 저장공간을 추가로 확보해야하는 시점을 지정하는 변수이다.
* 용량의 기본값은 16이며, 로드팩터의 기본값은 0.75이다. (저장공간의 75%가 채워졌을 떄 용량이 두배로 늘어난다.)

### 성능 문제?

HashSet의 성능은 초기 용량과 로드 팩터에 의해 결정된다.
* Set에 원소를 add 하는 과정은 일반적으로 시간복잡도 O(1)에 의해서 가능하지만, 최악의 경우는 O(n) 까지 나빠질 수 있어 HashSet의 초기 용량을 설정하는 것이 중요하다.
* 로드 팩터는 저장용량 대비 데이터를 이정도까지 채워야 탐색시간, 버킷 resize 등을 효율적으로 할 수 있다라고 미리 설정해놓는 것이다.
* 초기 용량을 작게 설정하면 메모리 측면에서는 좋지만, 버킷사이즈를 더 크게 만드는 과정이 있기 때문에 효율적이지 못하다
* 반면 초기 용량을 크게 설정하면 초기 메모리를 소모한다는데 단점이 있다.
 
## 중복을 걸러내는 과정

* HashSet의 add() 메서드는 새로운 요소를 추가하기 전 중복을 파악하기 위해 추가하려는 요소의 equals()와 hashCode()를 호출하기 때문에 목적에 맞게 오버라이딩해야 한다.
* hashCode()로 해시 코드를 얻어낸 다음 저장되어 있는 객체들의 해시 코드와 비교한다 
* → 그 후 같은 해시 코드가 있다면 equals() 메소드로 두 객체를 비교한다 
* → true가 나오면 동일한 객체로 판단하고 중복 저장을 하지 않는다.


