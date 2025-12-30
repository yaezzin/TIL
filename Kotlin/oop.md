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
* 코틀린에서는 필드만 만들면 getter, setter를 자동으로 만들어줌
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
