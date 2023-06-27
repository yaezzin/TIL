## Object.toString()과 String.valueOf()의 차이

알고리즘 문제를 풀던 와중 ch.toString() 과 String.valueOf(ch)처럼 형변환을 다르게 사용하는 것을 발견하였다. (변수 ch는 character형 변수명을 의미한다.) 그래서 이 둘의 차이가 무엇일지 궁금하여 정리해보았다.

두 메서드의 공통점은 ```모두 객체를 String값으로 변경한다```는 것이다.
하지만 ```변환 가능 자료형의 종류 및 개수```와 객체의 값이 ```null```일 경우 다른 모습을 보인다.



* ```String.valueOf()```의 경우에는 ```문자열 null```을 만들어서 반환을 한다.



### Object.toString()

>  ```Object.toString()```의 경우 객체의 값이 null이면 ```NullPointerException(NPE)```을 발생시킴

```java
public static void main(String[] args) {
    Object o = null;
    System.out.println(o.toString());
}

```
<img width="813" alt="스크린샷 2023-02-10 오후 11 27 59" src="https://user-images.githubusercontent.com/97823928/218116370-9b4fa646-ba8b-47f1-bfd4-31c442ec54f0.png">

> 또한 Object형 이외의 기본 자료형에도 사용 가능

<img width="432" alt="스크린샷 2023-02-10 오후 11 32 15" src="https://user-images.githubusercontent.com/97823928/218117331-99fbf2d5-e783-4e4e-b3fd-58a634ac1350.png">

### String.valueOf();

> ```String.valueOf()```의 경우 ```문자열 null```을 만들어서 반환함.

```java
public static void main(String[] args) {
    Object o = null;
    System.out.println(String.valueOf(o));
}
```

<img width="770" alt="스크린샷 2023-02-10 오후 11 27 38" src="https://user-images.githubusercontent.com/97823928/218116276-619e1806-d2de-4e58-91e6-e9b494007fd5.png">
* 결론적으로 toString()은 NPE를 발생시키므로 valueOf()가 더 안전한 선택이다.

