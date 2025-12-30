# Class

#### Java 코드
```java
public class Person {
    private final String name;
    private int age;

    public Person(String name, int age) {
        if (age <= 0) {
            throw new IllegalArgumentException(...)
        }
        this.name = name;
        this.age = age;
    }

    public Preson(String name) {
        this(name, 1);
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public boolean isAdult() {
        return this.age >= 20;
    }
}
```

#### Kotlin 코드
```kotlin
fun main() {
    val person = Person("이름", 100)
    println(person.name)
} 

class Person(val name: String, var age: Int) { // primary constructor
    init {
        if (age < 0) throw IllegalArgumentException("나이는 ${age}일 수 없습니다.")
    }

    constructor(name: String): this(name, 1) // secondary constructor - this로 호출

    // 함수를 프로퍼티처럼 사용 가능
    val isAdult: Boolean
        get() = this.age >= 20
}
```
* 코틀린에서는 필드만 만들면 getter, setter를 자동으로 만들어줌 - `프로퍼티(property)`
  * 자바로 만든 클래스를 사용하더라도 getter, setter 없이 출력 가능
* 자바에서는 private final String name;으로 선언했던 것을 한 줄로 변경 가능
* init (초기화) 블록은 생성자가 호출되는 시점에 호출
  * 값을 만들어주거나, validation 로직을 포함하는 용도로 사용
* `constructor`로 생성자 추가
  * 부생성자를 사용하기 보다는 정적 팩토리 메소드를 추천
 
#### backing field

```kotlin
class Person(name: String, var age: Int) {
    val name: String = name
        get() = field.uppercase()
}
```
* 외부에서 name을 부르면 get이 호출 -> get을 부르면 name에 대한 getter를 호출 -> 무한 루프 발생
* 따라서 무한루프를 막기 위한 예약어인 `field`를 사용 (실무에서의 사용은 드물다)

```kotlin
val upperCaseName: String
    get() = this.name.uppercase()
```

# Inheritance

#### Java Code

```java
public abstract class JavaAnimal {

    protected final String species;
    protected open int legCount; // open 키워드를 써야 override 가능

    public JavaAnimal(String species, int legCount) {
        this.species = species;
        this.legCount = legCount;
    }

    abstract public void move();
    public String getSpecies() { return species; }
    public int getLegCount() { return legCount; }
}

public class JavaCat extends JavaAnimal {
    public JavaCat(String species) {
        super(species, 4);
    }

    @Override
    public void move() {
        System.out.println("고양이 냥냥")
    }
}

public class JavaPenguin extends JavaAnimal {
    private final int wingCount;

    public JavaCat(String species) {
        super(species, 2);
        this.wingCount = 2;
    }

    @Override
    public void move() {
        System.out.println("펭귄 뒤뚱")
    }

    @Override
    public void getLegCount() {
        return super.legCount + this.wingCount;
    }
}
```

#### Kotlin Code

```kotlin
abstract class Animal(
    protected val species: String,
    protected val legCount: Int,
) {
    abstract fun move()
}

class Cat(
    species: String
) : Animal(species, 4) {

    override fun move() {
        println("고양이 냥냥")
    }
}

class Penguin(
    species: String
) : Animal(species, 2) {

    private val wingCount: Int = 2

    override fun move() {
        println("펭귄 뒤뚱")
    }

    override val legCount: Int
        get() = super.legCount + this.wingCount
}
```
* 상속 또는 구현할 때에 `:`로 를 사용
* 상위 클래스 상속을 구현할 때 생성자를 호출해야 함 - `Animal(species, 4)`
* 상속 관련 키워드
  * final : override를 할 수 없게 함 - default로 보이지 않게 존재
  * open : override를 열어 줌 (추상 멤버가 아니면 기보적으로 오버라이드가 불가능 하므로 open을 사용)
  * abstract : 반드시 override 해야 함
  * override : 상위 타입을 override하고 있음
* `주의` : 상위 클래스를 설게할 때 생성자 또는 초기화 블록에 사용되는 프로퍼티에는 open을 피해야 함

# Interface

#### Java Code

```java
public interface JavaSwimable {
    default void act() {
        ..
    }
}

public interface JavaFlyable {
    default void act() {
        ..
    }
}
```
* JDK8부터 default 메소드를 인터페이스에 넣을 수 있음

#### Kotlin Code

```kotlin
interfacee Swimable {
    val swimAbility: Int
        get() = 3

    fun act() {
        println(swimAbility)
        print("어푸어푸")
    }
}

interface Flyable {
    fun act() {
        ...
    }
}
```
* default 키워드 없이 메소드 구현이 가능

```kotlin
class Penguin(
    species: String
) : Animal(species, 2), Swimable, Flyable {

    private val wingCount: Int = 2

    override fun move() {
        println("펭귄 뒤뚱")
    }

    override val legCount: Int
        get() = super.legCount + this.wingCount

    override fun act() {
        super<Swimable>.act()
        super<Flyable>.act()
    }

    override val swimAblilty: Int
        get() = 3
}
```
* 중복되는 인터페이스를 특정할 때 **super<타입>.함수**를 사용

# Visibility Modifier

#### 자바와 코틀린의 차이

구분 | Java | Kotilin
-|-|-
public | 모든 곳에서 접근 가능| 모든 곳에서 접근 가능
protected |같은 패키지 또는 하위 클래스에서만 접근 가능| **선언된 클래스** 또는 하위 클래스에서만 접근 가능
default (internal) | 같은 패키지에서만 접근 가능 | 같은 모듈에서만 접근 가능 - `internal`
private | 선언된 클래스 내에서만 접근 가능 | 선언된 클래스내에서만 접근 가능

* 자바의 기본 접근 지시어 - `default`
* 코틀린의 기본 접근 지시어 - `public`
* 생성자 접근 지시어를 붙이려면 `constructor`를 사용해야함
* 자바는 같은 

#### 프로퍼티 접근 지시어

```kotlin
class Car(
    internal val name: String,
    _price: Int
) {
    var price = _price
        private set
}
```
* setter에만 추가로 가시성 부여가능

# object Keyword

## static method

```kotlin
class Person private constructor(
    var name: String,
    var age: Int,
) {
    companion object {
        private const val MIN_AGE = 1
        fun newBaby(name: String): Person {
            return Person(name, MIN_AGE)
        }
    }
}
```
* 코틀린에서는 static 키워드가 없기 때문에 companion object를 사용
  * `static` : 클래스가 인스턴스화 될 때 새로운 값이 복제되는 것이 아닌 정적으로 인스턴스끼리의 값을 공유
  * `companion object` : 클래스와 동행하는 유일한 오브젝트 
* const 변수를 붙이게 되면 컴파일 시에 변수가 할당 (붙이지 않으면 런타임 시 할당) - 진짜 상수에 붙이기 위한 용도

## Singleton


