# var & val

코틀린에서는 모든 변수에 수정 가능 여부를 명시해주어야 함
* var (variable, 가변)
* val (value, 불변)
* 모든 변수는 우선 val로 만든 후 필요한 경우에만 var로 바꾸는 것을 권장

# Type 

```kotlin
var number1 = 10L
var number1: Long = 10L
```
* 코틀린은 타입을 작성하지 않아도 되나, 명시적으로 작성도 가능

```kotlin
var number: Int
println(a) // 컴파일 에러 발생
```
* 초기값을 지정해주지 않는 경우 타입을 지정해주어야 함, 초기값을 지정하지 않고 값 사용 불가

```kotlin
val number1: Int = 4
val number2: Long = number1.toLong() 
println(number1 + number2)


val number3: Int? = 3
val number4: Long = number3?.toLong() ?: 0 //nullable 처리 필요
```
* 코틀린의 변수는 초기값을 보고 타입을 추론하며, 기본 타입들간의 변환은 명시적으로 이루어져야 함

```java
public static void printAgeIfPerson(Object obj) {
    if (obj isinstanceof Person) {
        Person person = (Person) obj;
        System.out.println(person.getAged());
    }
}
```
```kotlin
fun printAgeIfPerson(obj: Any) {
    if (obj is Person) {
        println(obj.age);
    }
}
```
* (Person) 타입 변환 없이 바로 사용 가능

#### Any
* Java의 Object 역할 (모든 객체의 최상위 타입)
* Any 자체로는 null을 표현하기 위해서 `Any?`를 사용

#### Unit
* Java의 void 역할이나, Kotlin에서는 실제 존재하는 타입임을 표현

#### Nothing
```kotlin
fun fail(message: String): Nothing {
    throw IllegalArgumentException(message)
}
```
* 함수가 정상적으로 끝나지 않았다는 사실을 표현
* 무조건 예외를 반환하는 함수, 무한 루프 함수 등


# nullable

####  safe call
```kotlin
val str: String? = "ABC"
str.length // 불가능
str?.length // 가능(세이프콜)
```
* 앞의 변수가 nullable이 아닌 경우에 함수를 실행 -> null인 경우 null 리턴

#### Elvis 연산자
```kotlin
val str: String? = "ABC"
str?.length ?: 0
```
* 앞의 연산 결과가 null인 경우 뒤의 값을 사용

#### null이 절대 아닐 경우

```kotlin
str!!.startsWith("A")
```
* 혹시나 null이 들어오면 NPE가 발생하므로 null아 아닌 것이 확실할 때만 `!!` 사용

#### Example 1

```java
// java 
public boolean startsWithA1(String str) {
    if (str == null) {
        throw IllegalArgumentException("Null value")
    }
    return str.startsWith("A")
}
```
```kotlin
fun startsWithA1(str: String?): Boolean {
    if (str == null) {
        throw IllegalArgumentException("Null value")
    }
    return str.startsWith("A")
}
```
```kotlin
fun startsWithA1(str: String?): Boolean {
    str?.startsWith("A") ? : throw IllegalArgumentException("Null Value")
}
```
* 타입에 null이 들어올 수 있으면 `?`사용

#### Example 2

```java
// java 
public Boolean startsWithA2(String str) {
    if (str == null) {
        return null
    }
    return str.startsWith("A")
}
```
```kotlin
fun startsWithA2(str: String?): Boolean? {
    if (str == null) {
        return false
    }
    return str.startsWith("A")
}
```
```kotlin
fun startsWithA2(str: String?): Boolean? {
    return str?.startsWith("A")
```

#### Example 3

```java
// java 
public boolean startsWithA3(String str) {
    if (str == null) {
        return false
    }
    return str.startsWith("A")
}
```
```kotlin
fun startsWithA2(str: String?): Boolean?{
    if (str == null) {
        return false
    }
    return str.startsWith("A")
}
```
```kotlin
fun startsWithA3(str: String?): Boolean? {
    return str?.startsWith("A") ? : false
```

# Operator

#### compareTo

* 자바와 다르게 코틀린은 객체를 비교할 때 비교연산자를 사용하면 자동으로 compareTo를 호출해줌 

#### 동등성, 동일성

* 자바에서는 동일성에 `==`, 동등성에 `equals`를 직접 호출
* 코틀린에서는 동일성에 `===`, 동등성에 `==`를 사용
  * == 를 사용하면 간접적으로 equals를 호출 



