# try-with-resources

try-with-resources문은 try에 자원 객체를 전달하면, try 코드 블록이 끝나면 자동으로 자원을 종료해주는 기능이다.
즉, 따로 finally 블록이나 모든 catch 블록에 종료 처리를 하지 않아도 된다.

```java
try (SomeResource resource = getResource()) {
    use(resource);
} catch(...) {
    ...
}
```
* 괄호 안에 객체를 생성하는 문장을 넣으면 따로 close()를 호출하지 않더라도 try문을 벗어나는 순간 자동적으로 close()가 호출된다.

하지만 자동으로 close()가 호출될 수 있으려면, 클래스가 ```AutoCloseable 인터페이스```를 구현한 것이어야만 한다.
```java
public interface AutoCloseable {
    void close() throws Exception;
}
```

## 자원반납 순서

자원 반납 순서의 경우 **나중에 선언된** 리소스부터 close(반납) 됩니다.

```java
public class CustomResource1 implements AutoCloseable {
 
    public CustomResource1() {
        System.out.println("CustomResource Constructor_1");
    }
 
    public void printMessage() {
        System.out.println("CustomResource Print Message_1");
    }
 
    @Override
    public void close() throws Exception {
        System.out.println("CustomResource close_1");
 
    }
}

public class CustomResource2 implements AutoCloseable {
 
    public CustomResource2() {
        System.out.println("CustomResource Constructor_2");
    }
 
    public void printMessage() {
        System.out.println("CustomResource Print Message_2");
    }
 
    @Override
    public void close() throws Exception {
        System.out.println("CustomResource close_2");
 
    }
}

public class ResourceClose {
    public static void main(String[] args) {
        try (CustomResource1 cr1 = new CustomResource1(); CustomResource2 cr2 = new CustomResource2()) {
            cr1.printMessage();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
#### 결과
```
CustomResource Constructor_1
CustomResource Constructor_2
CustomResource Print Message_1
CustomResource close_2
CustomResource close_1
```

ResourceClose 클래스는CustomResource1, CustomResource2 이렇게 2개의 리소스를 순서대로 생성하였다. 하지만 결과를 보면
* 리소스 생성은 CustomResource1, CustomResource2 순서로 되었지만,
* 리소스 자원 반납(close)는 CustomResource2, CustomResource1 순서로 된 것을 볼 수 있다.

## 출처
* [[Java] Try with resources 로 자원 반납하기](https://hianna.tistory.com/546)
