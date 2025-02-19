## 순환참조

순환참조란 두 개 이상의 빈이 서로 의존하고 있는 상태로, A가 B를 의존하고, B가 A를 다시 의존하는 형태
이때 스프링은 A와 B를 서로 주입하려고 하지만, 둘이 서로를 기다리므로 빈을 생성할 수 없게 됨

## @Lazy를 사용

```java
@Component
public class A {
    private B b;

    @Autowired
    public void setB(B b) {
        this.b = b;
    }
}

@Component
public class B {
    private A a;

    @Autowired
    public void setA(A a) {
        this.a = a;
    }
}
```

@Lazy를 한 쪽의 빈에 붙이면, 의존성 주입이 실제로 필요할 때까지 객체의 생성 시점을 지연시킴

## @Lazy를 추천하지 않는 이유

* 애플리케이션의 오류가 초기화 단계에서 발견되지 않고, 실제로 해당 빈이 사용될 때까지 기다려야 한다는 점
* 만약 잘못 구성된 빈이 지연 초기화 방식으로 초기화된다면, 애플리케이션 시작 시에는 오류가 발생하지 않지만, 해당 빈이 실제로 초기화될 때 문제가 발생
* 또한 JVM이 애플리케이션의 모든 빈을 적절히 메모리에 할당할 수 있도록 충분한 메모리를 갖추었는지 확인해야 함
* 이는 애플리케이션 시작 시 초기화되는 빈들만 고려하는 것이 아니라, 지연 초기화되는 빈들까지 포함하여 메모리 요구 사항을 고려해야 하기 때문

## 공식문서
https://docs.spring.io/spring-boot/reference/features/spring-application.html#features.spring-application.lazy-initialization
