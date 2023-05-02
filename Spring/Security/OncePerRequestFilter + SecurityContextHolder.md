## OncePerRequestFilter + SecurityContextHolder

`OncePerRequestFilter`는 스프링 시큐리티에서 제공하는 필터 클래스 중 하나이다.

- 이 필터는 HTTP 요청이 한 번만 필터링되도록 보장합니다. 즉, 동일한 요청에 대해 여러 번 필터링되는 것을 방지한다.
- OncePerRequestFilter는 추상 클래스이며, `doFilterInternal()` 메서드를 오버라이딩하여 구현한다.

```java
@Component
public class JwtTokenFilter extends OncePerRequestFilter {

    private final JwtTokenProvider jwtTokenProvider;

    public JwtTokenFilter(JwtTokenProvider jwtTokenProvider) {
        this.jwtTokenProvider = jwtTokenProvider;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws IOException, ServletException {
	      // 1. HTTP 요청 처리
				String token = resolveToken(request);
        try {
            if (token != null && jwtTokenProvider.validateToken(token)) {
                Authentication authentication = jwtTokenProvider.getAuthentication(token);
								// SecurityContext에 인증된 사용자 정보를 저장하여 이후 요청에서도 인증된 사용자에 대한 정보를 접근 가능
                SecurityContextHolder.getContext().setAuthentication(authentication);
            }
        } catch (BaseException e) {
            throw new RuntimeException(e);
        }

				// 2. 다음 필터 또는 서블릿에 요청 전달
        filterChain.doFilter(request, response);
    }

    private String resolveToken(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }
}
```

### SecurityContextHolder

Spring Security는 **인증된 사용자의 정보를 SecurityContext 객체에 저장**하며, 

이 SecurityContext 객체는 **SecurityContextHolder를 통해 언제든지 접근할 수 있다.**

- SecurityContextHolder는 ThreadLocal을 이용하여 각 스레드마다 독립적으로 SecurityContext 객체를 저장한다. 이렇게 함으로써 여러 사용자가 동시에 요청을 처리하더라도, 각 요청의 처리가 다른 요청과 상관없이 독립적으로 이루어질 수 있다.

✔️ **SecurityContextHolder의 세 가지 모드**

- `MODE_THREADLOCAL`: 기본 모드로, 각 스레드마다 SecurityContext 객체를 저장한다.
- `MODE_INHERITABLETHREADLOCAL`: ThreadLocal 대신 InheritableThreadLocal을 사용하여 SecurityContext 객체를 상속한다. 이 모드를 사용하면 하위 스레드에서도 부모 스레드의 SecurityContext 객체를 공유할 수 있다.
- `MODE_GLOBAL`: SecurityContext 객체를 정적 변수에 저장하여, 여러 스레드에서 공유합니다. 이 모드를 사용하면, 각 스레드의 인증 정보가 서로 덮어쓰일 수 있으므로, 보안에 취약할 수 있다.
