# CustomAccessDeniedHandler 403

AccessDeniedHandler는 스프링 시큐리티에서 **권한 부족으로 인한 접근 거부 시 호출되는 핸들러**이다. 

- 즉, **인증된 사용자가 요청한 리소스에 대한 권한이 없는 경우에 호출**된다. → 403 에러
- AccessDeniedHandler는 `AuthenticationException`과 `AccessDeniedException` 두 가지 예외를 처리한다.
- AuthenticationException은 인증 예외로, 인증된 사용자가 자격 증명에 실패한 경우 발생한다.
- AccessDeniedException은 인가 예외로, 인증된 사용자가 리소스에 대한 권한이 없는 경우 발생한다.

AccessDeniedHandler를 구현하려면 AccessDeniedHandler 인터페이스를 구현하면 된다. 

- 구현할 때, handle 메소드를 구현하여 AccessDeniedException과 HttpServletResponse 객체를 받아 처리할 수 있다.

```java
@Component
public class JwtAccessDeniedHandler implements AccessDeniedHandler {

    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response,     AccessDeniedException accessDeniedException) throws IOException, ServletException {
        response.sendError(HttpServletResponse.SC_FORBIDDEN);
    }
}
```

이후 SecurityConfig에 다음과 같이 설정해주면 된다.
