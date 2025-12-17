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


# 조건문

#### example 1 
```java
private void validateScoreIsNotNegative(int score) {
    if (score < 0) {
        throw new Exception()...
    }
}
```

```kotlin
fun validateScoreIsNotNegative(score: Int) {
    if (score < 0) {
        throw Exception...
    }
}

```
* new를 사용하지 않고 예외 던짐


#### Expression과 Statement

* Statement : 프로그램의 문장, 하나의 값으로 도출되지 않음
* Expression: 하나의 값으로 도출
* 자바에서 if-else는 Statement이지만 Kotlin에서는 Expression

```kotlin
fun getPassOrFail(score: Int): String {
    return if (score >= 50) {
        "P"	
    } else {
        "F"
    }
}
```
* 코틀린에서는 statement로 간주하기 때문에 If부터 else 블락까지 전체를 한 번에 리턴할 수 있음
* 코틀린에서는 if-elsefmf expression으로 사용할 수 있기 때문에 3항 연산자가 존재하지 않음

#### swith

```java
private String getGrade(int score) {
    switch (score / 10) {
        case 9:
            return "A";
        case 8:
            return "B";
        else:
            return "D"
    }
}
```

```kotlin
fun getGrade(score: Int): String {
    return when (score / 10) {
        9 -> "A"
        8 -> "B"
        in 6..7 -> "C" // 이런식으로 범위도 가능!
        else -> "D"
    }
}
```
* 자바에서의 switch를 when으로 표현 가능
* statement이기때문에 바로 리턴 가능


```java
private boolean startsWithA(Object obj) {
	if (obj instanceof String)
		return ((String) obj).startsWith("A");
	else 
		return false;
}
````

```kotlin
fun startsWithA(obj: Any): Boolean {
	return when(obj) {
		is String -> obj.startsWith("A")
		else -> false
	}
}
```

# 반복문 

#### for-each

```kotlin
val numbers = listOf(1L, 2L, 3L) 
    for (number in numbers) { 
        println(number)
	}
}
```
* 자바와 동일하게 Iterable이 구현된 타입이라면 모든지 들어갈 수 있음
* `in`을 사용

#### for문

```kotlin
for (i in 1..3) {
    println(i)
}
```
* `..`를 사용해서 범위를 나타냄

```kotlin
for (i in 3 downTo 1) {
    println(i)
}
```
* `downTo` 신기하당

```kotlin
for (i in 1..5 step 2) {
    println(i)
}
```
* 1부터 5까지 2 `step`씩 올라가는 것을 나타냄


#### Progression과 Range

<img width="336" height="312" alt="스크린샷 2025-12-17 오후 2 57 02" src="https://github.com/user-attachments/assets/7bd85da0-3ccd-480c-aa69-eb646d1f7fb8" />

* ..연산자는 IntRange 객체를 반환하며, IntRange는 IntProgression을 상속받고 있음
* IntProgression을 상속받을 때, step값을 디폴트 1로 세팅하고 있음 그래서 ..연산자가 1씩 증가하도록 동작함

# 예외 

#### try-catch-finally

```kotlin
fun parseIntOrThrow(str: String): Int? {
	return try {
    	str.toInt()
    } catch (e: NumberFormatExpression) {
   		null
    }
}
```
* 자바와 코틀린 거의 동일하게 작성하나 코틀린은 expression!
* 타입이 뒤에 위치하고 new를 사용하지 않음
  
#### checked/unchecked exception

* 코틀린에선 checked exception과 unchecked exception을 구분하지 않음. 모두 unchecked exception임
* 따라서 throws를 지정해주지 않아도 됨

#### try with resources

```try with resources```
* try 블록에 괄호()를 추가하여 파일을 열거나 자원을 할당하는 명령문을 명시하면, 해당 try 블록이 끝나자마자 자동으로 파일을 닫거나 할당된 자원을 해제해주는 구문

```java
public boid readFile(String path) throws IOException {
	try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
    	System.out.println(reader.readLine());
    }
}
```
* try-with-resources 구문은 JDK7부터 추가됐으나 코틀린엔 try with resources 구문이 없다!
* 대신, 코틀린의 특성을 이용해서 만든 use라는 inline 확장함수를 사용하면 위 구문과 같은 결과를 낼 수 있음.

```kotlin
fun readFile(path: String) {
	BufferedReader(FileReader(path)).use {
    	reader -> println(reader.readLine())
    }
}
```

# 함수 

```kotlin
fun max(a: Int, b: Int) =
    if (a > b) {
        a
    } else {
        b
    }

fun max(a: Int, b: Int) = if (a > b) a else b // 한줄로도 가능
```
* 접근 지시어 생략 가능
* 함수를 표시하는 키워드 fun
* 함수가 하나의 결과값이면 block`{ .. }` 대신 `=` 사용가능
* block { }을 사용하는 경우 반환타입이 유닛이 아닌 이상 명시적으로 작성해주어야 합니다


```java
public void repeat(String str, int num, boolean useNewLine) {
	// 반복 수행 로직
}

public void repeat(String str, int num) {
	repeat(str, num, ture);
}

public void repeat(String str) {
	repeat(str, 3, true);
}
```
* 오버로딩 - 이러한 코드가 메소드 중복처럼 느껴진다면, 코틀린에선 이 문제를 default parameter로 해결할 수 있음!
  
```kotlin
fun repeat(
	str: String,
    num: Int = 3,
    useNewLine: Boolean = true
) {
	// 반복 수행 로직
}
```

#### named argument

만약 repeat을 호출할 때 num은 3을 그대로 쓰고 UseNewLine은 false를 쓰고 싶다면?
* 호출 부에서 repeat("Hello World", 3, false) 처럼 함수 호출 시에 직접 지정해주면 됨
* 매개변수 이름을 통해 직접 지정하고, 지정되지 않은 매개변수는 기본값을 사용

```repeat(str="Hello World", num=3, useNewLine=false)```
* Builder를 만들지 않고도 Builder의 효과를 가질 수 있음


#### 가변 인자

```java
public static void printAll(String... strings) {
    for (String str : strings) {
        System.out.println(str);
    }
}
```
* 타입..을 쓰면 가변인자 사용
* 호출 부에서는 printAll(array) 또는 printAll("A", "B", "C")와 같이 사용

```kotlin
//호출
val array = arrayOf("A", "B", "C")
printAll(*array)

fun printAll(varars strings: String) {
    for (str in strings) {
        println(str)
    } 
}
```
* ... 대신 vararg 사용하며 배열을 바로 넣는 대신 스프레드 연산자 *를 붙여줘야함
