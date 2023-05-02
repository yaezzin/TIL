# UserDetailsService

```java
@RequiredArgsConstructor
@Service
public class CustomUserDetailsService implements UserDetailsService {

    private final MemberRepository memberRepository;

    @Override
    public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
        UserDetails userDetails = memberRepository.findByEmail(email)
                .map(this::createUserDetails)
                .orElseThrow(() -> new UsernameNotFoundException(email + "데이터베이스에서 찾을 수 없습니다."));
        return userDetails;
    }

    private UserDetails createUserDetails(Member member) {
        GrantedAuthority grantedAuthority = new SimpleGrantedAuthority(member.getAuthority().toString());
        return new User(
                member.getEmail(),
                String.valueOf(member.getPassword()),
                Collections.singleton(grantedAuthority)
        );
    }
}
```

### 1. UserDetailsService

UserDetailsService는 Spring Security에서 인증 과정에서 사용되는 인터페이스입니다. 이

- 인터페이스를 구현하면, 인증 과정에서 사용되는 유저 정보를 가져오는 작업을 수행할 수 있습니다.
- UserDetailsService는 **loadUserByUsername** 메서드를 가지고 있습니다.
- 이 메서드는 사용자의 이름(이메일, 아이디 등)을 파라미터로 받아서, 해당 사용자의 정보를 담고 있는 UserDetails 객체를 반환하는 역할을 합니다.

### 2. SimpleGrantedAuthroity

SimpleGrantedAuthority는 스프링 시큐리티에서 **권한 정보를 나타내는 클래스이다.**

- `GrantedAuthority` 인터페이스를 구현하며, 권한의 이름을 저장하는 역할을 한다.
- 예를 들어, **"ROLE_ADMIN"**과 같은 문자열을 SimpleGrantedAuthority 객체로 생성하여 권한 정보를 나타낼 수 있다.
