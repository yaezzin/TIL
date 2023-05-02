## 지네릭스

지네릭스는 다양한 타입의 객체를 다루는 메서드나 컬렉션 클래스에 ```컴파일 시의 타입체크```를 해주는 기능이다.
* 컴파일 시 객체의 타입을 체크하므로 ```타입 안정성```을 높인다
  * 의도하지 않은 타입의 객체가 저장되는 것을 막고,
  *  저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변한되어 발생할 수 있는 오류를 줄인다. 
* 타입체크와 형변환을 생략할 수 있으므로 ```코드가 간결```해진다.

###  1. 선언
클래스 옆에 ```<T>```를 붙이면 된다.
* Box<T>에서 ```<T>```를 ```타입 변수```라고 한다.

```java
class Box<T> {
    T item;

    void setItem(T item) { 
        this.item = item
    }

    T getItem() {
        return item;
    }
}
```

선언시 <> 안에 어떤 문자가 들어가도 상관 없으나 상황에 맞게 의미있는 문자를 선택해서 사용하는 것이 좋다.
* ```E``` : 요소(Element, 자바 컬렉션(Collection)에서 주로 사용됨
* ```K``` : 키
* ```N``` : 숫자
* ```T``` : 타입
* ```V``` : 값
* ```S, U, V``` : 두 번째, 세 번째, 네 번째에 선언된 타입

### 2. 생성

객체를 생성할 떄는 다음과 같이 참조변수와 생성자에 타입 T 대신 실제 타입을 지정해주어야 한다.

```java
Box<String> b = new Box<String>();
b.setItem(new Object); // 에러. String 이외의 타입은 지정불가
b.setItem("abc");
String item = b.getItem(); // (String) b.getItem(); 처럼 형변환 필요 없음
```

### 3. 지네릭스의 제한

#### 3-1. static

```java
class Box<T> {
    static T item; // 에러
}
```
static은 모든 객체에 동일하게 동작해야하기 때문에 static 멤버에 타입 변수 T를 사용할 수 없다.

#### 3-2. 배열 생성

```java
class Box<T> {
    T[ ] itemArr; // 
    T[ ] toArray() {
        T[] tmpArr = new T[itemArr.length]; //에러. 지네릭 배열 생성불가
        ...
        return tmpArr;
    }
}
```

지네릭 배열 타입의 참조변수를 선언하는 것은 가능하지만 new T[10];와 같이 배열을 생성하는 것은 불가능하다
* new 연산자는 컴파일 시점에 타입 T가 무엇인지 정확히 알아야 하므로 지네릭 배열 생성이 불가능 하다.
* 지네릭 배열을 생성해야할 필요가 있을 때는 Reflection ApI의 newInstance()와 같이 동적으로 객체를 생성하는 메서드로 배열을 생성하거나, Object 배열을 생성하여 복사한 다음에 T[]로 형변환하는 방법을 사용한다.

### 4. 지네릭 클래스의 객체 생성과 사용

```java
class Box<T> {
    ArrayList<T> list = new ArrayList<T>();
    void add(T item) { list.add(item); }
    ....
}
```
지네릭 클래스가 다음과 같이 정의되어 있다고 가정하자.

```java
Box<Apple> appleBox = new Box<Apple>();
Box<Apple> grapeBox = new Box<Grape>(); // 에러
````
객체 생성 시에는 참조변수와 생성자에 대입된 타입이 일치해야한다!

```java
Box<Fruit> appleBox = new Box<Apple>(); // 에러
````
두 타입이 상속관계(Fruit - Apple)이더라도 타입이 일치해야 한다.

```java
Box<Apple> appleBox = new FruitBox<Apple>(); // 가능
````
하지만 두 지네릭 클래스의 타입이 상속관계에 있고, 대입된 타입이 같은 것은 가능하다.

```java
Box<Apple> appleBox = new Box<Apple>();
appleBox.add(new Grape()); // 에러
appleBox.add(new Apple()); // 가능
```
add 메서드에 객체를 추가할 때 대입된 타입과 다른 타입의 객체는 추가할 수 없다

```java
Box<Fruit> fruitBox = new Box<Fruit>();
fruitBox.add(new Fruit()); // 가능
fruitBox.add(new Apple()); // 가능
```
타입 T가 Fruit인 경우 Fruit의 자손들은 메서드의 매개변수가 될 수 있다.

### 5. 제한된 지네릭 클래스

```java
class FruitBox<T extends Fruit> { // Fruit의 자손만 타입으로 지정가능
    ...
}
```
타입문자로 사용할 타입을 명시하면 한 종류의 타입만 저장할 수 있도록 제한할 수 있지만 그래도 여전히 모든 종류의 타입을 지정할 수 있다.
* 하지만 ```extends``` 키워드를 통해 타입 매개변수 T에 특정 타입의 자손들만 대입하도록 제한할 수 있다.
* 만일 클래스 대신 인터페이스를 구현해야한다는 제약이 필요하더라도 implements가 아닌 extends를 사용한다.

```java
FruitBox<Apple> appleBox = new FruitBox<Apple>(); // 가능
FruitBox<Toy> toyBox = new FruitBox<Toy>(); // 에러

FruitBox<Fruit> fruitBox = new FruitBox<Fruit>(); // 가능
fruitBox.add(new Apple()); // 가능 
fruitBox.add(new Grape()); // 가능
```
* add()의 매개변수 타입도 Fruit과 그 자손 타입이 될 수 있다.
