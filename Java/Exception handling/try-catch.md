# try - catch문

## 예외처리 (Exception Handling)
프로그램 실행 시 발생할 수 있는 예외에 대한 코드를 작성하여 프로그램의 비정상 종료를 막고 정상적인 실행상태를 유지하기 위해 작성한다.
* 발생한 예외를 처리하지 못하면 프로그램은 비정상적으로 종료되며, ```JVM의 예외처리기```가 받아서 예외의 원인을 화면에 출력한다.

```java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣음
} catch(Exception e) {
   // 예외 발생 시 이를 처리하기 위한 문장들
} catch(Exception e) {
   // 예외 발생 시 이를 처리하기 위한 문장들
}
```
* 하나의 메서드 내에 여러개의 try-catch 문을 사용할 수 있음
  * 위의 모든 catdh블럭에 참조변수 e 하나만의 사용해도 된다. 
  * 하지만 중첩된 try-catch문의 경우 다른 참조변수 이름을 사용해야 한다.
* 흐름 
  * try 블럭 내에서 예외가 발생하면 발생한 예외와 일치하는 catch블럭을 찾는다. 예외가 발생하지 않는다면 catch블럭을 거치지 않는다.
  * 일치하는 catch 블럭이 있으면 그 블럭 내의 문장들을 수행한다.
  * 일치하는 catch 블럭이 없다면 예외가 처리되지 못한다.
  
```java
try {
    System.out.println(0/0); 
} catch(ArithmeticException e) {

} catch(Exception e) {

}
```
* try 블럭에서 ArithmeticExcpetion이 발생하였고, 첫번째 검사에서 일치하는 catch블럭을 찾았기 때문에 두번째 catch은 찾지 않게 된다.
* 그렇기 때문에 마지막에 Exception 클래스 타입의 참조변수를 선언한 catch문을 사용하면 어떤 종류의 예외가 발생하더라도 이 블럭에 의해서 처리될 수 있다. 
