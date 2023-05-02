# UsernamePasswordAuthenticationToken이 뭘까?

→ 로그인 코드를 짤 때 UserService에 다음과 같이 작성했다. 이 코드를 분석해보자.

```java
UsernamePasswordAuthenticationToken authenticationToken = 
				new UsernamePasswordAuthenticationToken(member.getEmail(), member.getPassword());

Authentication authentication = authenticationManagerBuilder.getObject().authenticate(authenticationToken);

TokenResponse tokenResponse = jwtTokenProvider.createToken(authentication);
```

### 1. UsernamePasswordAuthenticationToken

```java
public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {
    private static final long serialVersionUID = 
													SpringSecurityCoreVersion.SERIAL_VERSION_UID;

    private final Object principal;
    private Object credentials;

    public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
        super(null);
        this.principal = principal;
        this.credentials = credentials;
        setAuthenticated(false);
    }

    public UsernamePasswordAuthenticationToken(Object principal, Object credentials,
            Collection<? extends GrantedAuthority> authorities) {
        super(authorities);
        this.principal = principal;
        this.credentials = credentials;
        super.setAuthenticated(true); // must use super, as we override
    }

    // ...생략

    @Override
    public Object getCredentials() {
        return this.credentials;
    }

    @Override
    public Object getPrincipal() {
        return this.principal;
    }
}
```

- `UsernamePasswordAuthenticationToken` 클래스는 `AbstractAuthenticationToken` 클래스를 상속받아 구현되어 있다.
- `UsernamePasswordAuthenticationToken` 객체는 사용자 이름과 암호를 인자로 받아서 생성되며, 이 객체는 사용자의 인증 정보를 나타낸다. → 인증이 성공하면 인증된 사용자 정보를 담고 있는 객체로 반환됨.

### 2. AuthenticationManagerBuilder

AuthenticationManagerBuilder는 사용자 인증 및 인가를 구성하기 위한 빌더 클래스이다.

```java
Authentication authentication = 
     authenticationManagerBuilder.getObject().authenticate(authenticationToken);
```

- AuthenticationManager 인터페이스를 구현한 객체를 getObject()로 가져오고,
- UsernamePasswordAuthenticationToken 객체를 파라미터로 받아 사용자 인증을 수행하고
- Authentication 객체를 반환한다.
- 반환된 Authentication 객체를 TokenProvider의 createToken() 메서드에 전달하면 액세스 토큰이 발행!!
