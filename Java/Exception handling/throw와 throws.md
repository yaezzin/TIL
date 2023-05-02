# throw와 throws
## throw

키워드 ```throw```를 사용하면 프로그래머가 **고의로 예외를 발생**시킬 수 있다.
```java
1. new를 통해 발생시키려는 예외클래스의 객체를 만든다.
    Exception e = new Excpetion("고의로 발생시킴");
2. throw를 통해서 예외를 발생시킴
    throw e;
3. throw new Exception("고의로 발생시킴"); 처럼 한 줄로 줄여 쓸 수 있다.
```

## throws

throws를 사용해서 자신을 호출한 상위 메소드로 에러를 던질 수 있다.

```java
void method() throws Exception {
    // 메서드 내용
}
```
* 메서드의 ```선언부에 에외를 선언```함으로써 메서드 사용자가 선언부를 보았을 때 어떠한 예외들이 처리되어야 하는지 쉽게 파악할 수 있다.
* 메서드에 예외를 선언할 때 일반적으로 RuntimeException 클래스들은 적지 않는다. (unchecked 에러이기 떄문에 문제가 되지 않는다)
* 그러므로 반드시 처리되어야 하는 예외들만 선언해주는 것이 좋다.

앞서 ```throws를 사용해서 자신을 호출한 상위 메소드로 에러를 던질 수 있다```라고 하였는데,

```java
class ExceptionEx12 {
	public static void main(String[] args) throws Exception {
		method1(); // 같은 클래스내의 static멤버이므로 객체생성없이 직접 호출가능.
  	}

	static void method1() throws Exception {
		method2();
	}	

	static void method2() throws Exception {
		throw new Exception();
	}	
}

출력값
Exception in thread "main" java.lang.Exception
        at ExceptionEx12.method2(ExceptionEx12.java:11)
        at ExceptionEx12.method1(ExceptionEx12.java:7)
        at ExceptionEx12.main(ExceptionEx12.java:3)
```
*  이러한 식으로 계속 호출스택에 있는 메서드들을 따라 전달되다가 제일 마지막에 있는 main 메서드에서도 예외처리가 되지 않으면 main 메서드마저 종료되어 프로그램이 전체 종료된다.
* 이것으로 예외가 처리된 것이 아니라 단순히 예외를 전달만 한 것이기 때문에 어느 한 곳에서 try-catch문으로 예외처리를 해주어야 한다.

### 방법1 

```java
public static void main(String[] args) {
    method1(); 
 }

static void method1() {
    try {
        throw new Exception(); // 고의로 예외 발생
    } catch (Exception e) {
        e.printStackTrace();
    }
}	
```

### 방법2

```java
public static void main(String[] args) {
    try {    
        method1(); 
    } catch (Exception e) {
        e.printStackTrace();
    }
}

static void method1() throws Exception {
    throw new Exception(); // 고의로 예외 발생
}	
```

### 차이 ?

방법 1은 메서드 1에서 예외처리를 한 반면, 방법 2는 method1에서 예외를 선언하여 자신을 호출하는 main메서드에서 예외처리를 했다.
분명 어디서든 예외처리를 하면되는데 무슨 차이가 있을까?

먼저, 방법1의 경우에는 메서드 내에서 예외가 처리되어지면 메서드1을 호출한 메서드는 예외가 발생했다는 사실 조차 모르게 된다.
하지만 방법2처럼 호출한 메서드로 넘겨주면 메서드를 호출한 라인에서 예외가 발생한 것으로 간주된다.
