# SecurityConfigurerAdapter

```java
@RequiredArgsConstructor
public class JwtConfigurer extends SecurityConfigurerAdapter<DefaultSecurityFilterChain, HttpSecurity> {

    private final JwtTokenProvider jwtTokenProvider;

    @Override
    public void configure(HttpSecurity http) {
        JwtTokenFilter customFilter = new JwtTokenFilter(jwtTokenProvider);
        http.addFilterBefore(customFilter, UsernamePasswordAuthenticationFilter.class);
    }
}
```

SecurityConfigurerAdapter는 Spring Security의 구성 클래스 중 하나로, 이 클래스를 상속받아 사용하면, Spring Security의 구성을 좀 더 유연하게 할 수 있습니다.

- SecurityConfigurerAdapter 클래스는 WebSecurityConfigurerAdapter나 GlobalSecurityConfigurerAdapter와 같은 다른 구성 클래스와 함께 사용됩니다.
- 보통 SecurityConfigurerAdapter는 보안 필터 체인에 필터를 추가하거나, 사용자 인증을 위한 인증 제공자를 구성하거나, CSRF 보호를 구성하는 등의 구성 작업을 수행합니다. 또한 구성 작업을 모듈화하여 코드의 재사용성을 높이고, 가독성을 높이며, 코드의 복잡성을 줄일 수 있습니다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private JwtTokenProvider jwtTokenProvider;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/api/public/**").permitAll()
                .antMatchers("/api/private/**").authenticated()
                .and()
                .apply(new JwtConfigurer(jwtTokenProvider));
    }
		// 생략  ..
}
```

**`/api/public/**`** 엔드포인트에 대해서는 인증을 건너뛰고, 

**`/api/private/**`**엔드포인트에 대해서는 인증이 필요하도록 설정합니다. 

또한 **`JwtConfigurer`** 클래스를 사용하여 **JWT 토큰을 인증하는 필터(JwtTokenProvider)를 보안 필터 체인에 추가**합니다. 

이렇게 함으로써, **JWT 토큰이 필요한 API 엔드포인트에 대해서는 토큰의 유효성 검사를 통해 인증을 수행할 수 있습니다.**
