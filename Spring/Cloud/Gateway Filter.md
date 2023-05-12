# Filter

<img width="600" alt="스크린샷 2023-05-12 오후 3 50 00" src="https://github.com/yaezzin/TIL/assets/97823928/b59953fa-c05d-4e60-bbd6-90943d0bf0b0">

### GatewayHandlerMapping

클라이언트의 요청이 들어오면 GatewayHandlerMapping가 해당 요청을 어떤 라우트로 보낼지 결정한다. 
* Spring MVC의 HandlerMapping과 유사한 역할
* GatewayHandlerMapping은 라우트 구성 정보를 참조하며, 요청의 URI, HTTP메서드, 헤더 등을 기반으로 **라우팅**

#### FilterConfig

```java
@Configuration
public class FilterConfig {

    @Bean
    public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
        return builder.routes()
            .route(r -> r.path("/member-service/**")
                 .filter(f -> f.addRequestHeader("member-request", "member-request-header")
                         .addResponseHeader("member-response", "member-response-header"))
                 .uri("http://localhost:8081/"))
            // ...
            .build();
    }
}
```

### Predicate

* 요청의 URI, HTTP 메서드, 헤더, 쿼리 매개변수 등과 같은 요청 정보를 이용하여 요청을 필터링하고, 라우팅할지 여부를 결정

### PreFilter, PostFilter

* Spring Cloud Gateway에서 제공하는 GatewayFilter의 구현체 중 하나로, 각각 GatewayFilter가 실행되기 이전과 이후에 요청/응답을 가로채어 처리하는 역할을 수행
* ```PreFilter``` : GatewayFilter가 실행되기 이전에 요청을 가로채어 처리, 예를 들어, 요청 헤더나 바디를 변경하는 작업을 할 수 있음
* ```PostFilter``` : GatewayFilter가 실행된 후에 응답을 가로채어 처리, 예를 들어, 응답 헤더나 바디를 변경하거나 로깅 작업을 할 수 있음
