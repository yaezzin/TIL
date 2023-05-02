## printStackTrace()란

예외가 발생했을 때 생성되는 예외 클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨져 있다.
이러한 정보들은 ```getMessage()```와 ```printStackTrace()```를 통해 얻을 수 있다.

```java
class ExceptionEx { 
    ... 생략
    try {
        System.out.println(0/0); 
    } catch(ArithmeticException e) {
        e.printStackTrace();
       System.out.println("예외 메세지 : " + e.getMessage());
    }
}
```

#### 실행결과
```java
java.lang.ArithmeticException: / by zero at ExceptionEx.main(ExceptionEx.java:3)
예외 메세지 : /by zero
```

* printStackTrace()를 사용해서 호출스택에 대한 정보와 예외 메세지를 출력할 수 있다.
* 에러의 발생근원지부터 ```단계별```로 출력

## 지양하는 이유 ?
* printStackTrace()를 사용하게 되면 패키지, 클래스 및 오류의 종류까지 전부 출력을 하게 된다.
  * 공격자는 해당 로그를 탈취하여 각종 클래스 및 정보를 획득할 수 있으며, 내부 구조를 쉽게 파악할 수 있다.
  * 그러므로 개발 서버에서는 사용해도 되나 운영서버로 코드를 배포할 때는 예외를 log로 처리하는 것이 좋다.
* JAVA의 Reflection을 이용하여 예외를 추적하는 것이라 많은 오버헤드가 발생할 수 있다.
* 메서드 스택정보를 취합하기 때문에 서버 부하의 원인이기도 하다.
* 내부적으로 System.err을 사용하는데 비용이 비싼편이다. 

## 출처
* [Spring - e.printStackTrace() 보다 StackTraceElement로 로깅하기](https://kim-jong-hyun.tistory.com/112#:~:text=%EA%B7%B8%EB%9F%AC%EB%82%98%20e.printStackTrace()%EB%8A%94,%EB%B6%80%ED%95%98%EC%9D%98%20%EC%9B%90%EC%9D%B8%EC%9D%B4%EA%B8%B0%EB%8F%84%20%ED%95%98%EB%8B%A4.)
