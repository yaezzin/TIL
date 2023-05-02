# AuthenticationEntryPoint 401

```java
@Component
public class JwtAuthenticationEntryPoint implements AuthenticationEntryPoint {
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException {
        response.sendError(HttpServletResponse.SC_UNAUTHORIZED);
    }
}
```

`AuthenticationEntryPoint`는 **인증되지 않은 사용자가 보호된 리소스에 액세스하려고 할 때** 어떻게 처리할지를 결정하는 인터페이스입니다.  → 401 에러

- 인증되지 않은 사용자가 보호된 리소스에 액세스하려고하면 AuthenticationEntryPoint가 실행된다.
- **commence()** 메서드는 인증되지 않은 사용자가 보호된 리소스에 액세스하려고 시도할 때 호출됩니다. 이 메서드는 HttpServletRequest, HttpServletResponse, AuthenticationException 매개변수를 사용하여 호출됩니다.
- commenct() 메서드는 인증되지 않은 사용자가 보호된 리소스에 액세스하려고 할 때 로그인 페이지로 리디렉션하거나, 인증되지 않은 사용자에게 적절한 오류 메시지를 반환할 수 있습니다.
