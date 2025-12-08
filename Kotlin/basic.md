## var & val

코틀린에서는 모든 변수에 수정 가능 여부를 명시해주어야 함
* var (variable, 가변)
* val (value, 불변)
* 모든 변수는 우선 val로 만든 후 필요한 경우에만 var로 바꾸는 것을 권장

## type 

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

```
// java 
final List<Integer> numbers = Arrays.asList(1, 2);
numbers.add(3)
```
* 컬렉션 자체를 변경하지 못하더라도, 원소 추가는 가능

코틀린에서는 primitive type과 reference type의 구분이 없음
* 코틀린이 내부적으로 상황에 따라 처리해줌 (boxing, unboxing을 고려할 필요가 없음)

## nullable

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
