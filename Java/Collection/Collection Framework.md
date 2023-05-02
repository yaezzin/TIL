## 컬렉션 프레임웍

<img width="357" alt="스크린샷 2023-01-17 오후 5 47 10" src="https://user-images.githubusercontent.com/97823928/212851104-bfd4907e-baac-453c-a9da-bc5d2ab40879.png">

```컬렉션 프레임웍```이란 데이터 집합을 저장하는 클래스들을 표준화한 설계를 뜻한다.
* 컬렉션 프레임웍이 등장함에 따라 다양한 컬렉션 클래스가 추가되고, 모든 컬렉션 클래스를 표준화된 방식으로 다룰 수 있도록 체계화되었다.
* 컬렉션 프레임웍은 ```List```, ```Set```, ```Map``` 3가지 인터페이스를 정의하였고, List와 Set의 공통부분만 뽑아 다시 새로운 인터페이스인 ```Collection```을 추가로 정의하였다.


### 1. List 인터페이스 

* List 인터페이스는 **중복을 허용**하면서, **저장순서가 유지**되는 컬렉션을 구현하기 위해 사용된다.
* 배열의 경우 선언된 공간 이외에는 사용할 수 없는데, 이러한 배열의 단점을 보완하여 List를 통해 구현된 클래스들은 ```동적 크기```를 가지면서 배열처럼 사용할 수 있다.
* 구현 클래스 : ArrayList, LinkedList, Stack, Vector

### 2. Set 인터페이스

* Set 인터페이스는 중복을 허용하지 않고 저장순서가 유지되지 않는 컬렉션 클래스를 구현하는데 사용한다.
* Set은 일반적으로 입력받은 순서와 상관없이 데이터를 집합시키기 때문에 입력받은 순서를 보장할 수 없다.
  * 만약 중복은 허용하고 싶지 않으나 순서를 보장받고 싶다면 LinkedHashSet을 사용해야 한다.
* 구현 클래스 : HashSet, SortedSet, TreeSet

### 3. Map 인터페이스

<img width="610" alt="스크린샷 2023-01-17 오후 6 08 26" src="https://user-images.githubusercontent.com/97823928/212855994-9229bd90-9670-4a8c-9c75-0be3e89d9897.png">

* Key와 Value를 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는데 사용한다.
* Key는 중복될 수 없으나 Value는 중복을 허용한다.
* 구현 클래스 : Hashtable, HashMap, LinkedHashMap, SortedMap, TreeMap 등
* values()에서 반환타입이 Collection이고 keySet()에서는 반환타입이 Set인 것을 주목하자
  * Map 인터페이스는 값의 중복을 허용하므로 Collection타입으로 반환하고, 키의 중복을 허용하지 않으므로 Set으로 반환한다.

### 4. Map.Entry 인터페이스

* Map.Entry 인터페이스는 Map 인터페이스의 내부 인터페이스이다.
* Map에 저장되는 Key-Value 쌍을 다루기 위해 내부적으로 Entry 인터페이스를 정의하였다.

