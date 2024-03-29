## Java의 main 메서드가 static인 이유

### 왜 우리는 메인메서드에 static을 붙일까 ?
```java
public static void main(String[] args) {
    ...
}
```

#### static

* static 으로 선언한 함수나 변수는 JVM에서 ```객체의 생성 없이``` 메모리에 할당시켜 호출 가능한 형태로 만듦
* new를 통해 객체를 생성할 필요 없이 ```클래스이름.static변수/함수```의 형태로 어디서든지 호출이 가능

### Non-static

* 하지만 static이 아닌 경우 객체가 생성되는 런타임(Runtime)시에 메모리에 할당되어 참조값(Reference)를 통하여 접근할 수 있다. 
* 즉, 일반 메서드는 객체가 new 키워드를 이용해 생성이 되야 객체가 메모리에 할당되어 접근/호출이 가능하다. 

```java
 메인클래스 변수 = new 메인클래스(); 
 변수.main();
```
* 만약 main 메서드에 static이 붙지 않는다면 다음과 같이 new를 통해 메인 클래스의 객체를 생성한 이후에 main() 메서드를 호출해주어야 한다. 
* main 메서드는 프로그램이 실행되면 제일 먼저 호출되는 메서드이기 때문에 객체를 생성하지 않은 채로 바로 작업을 수행해야 하므로 static이어야 한다!
