# TokenProvider

토큰 기반 인증 방식에서 **토큰 생성과 검증을 담당**하는 인터페이스

- 클라이언트가 요청을 보낼 때, **Filter는 TokenProvider를 호출**하여 토큰을 생성하거나 검증한다.
- 이후, Filter는 Authentication 객체를 생성하여 Spring Security의 인증 처리를 수행한다.

```java
public TokenResponse createToken(Authentication authentication) {
		String authorities = authentication.getAuthorities().stream()
		    .map(GrantedAuthority::getAuthority)
        .collect(Collectors.joining(","));

    long now = (new Date()).getTime();
    Date accessTokenExpiresIn = new Date(now + ACCESS_TOKEN_EXPIRE_TIME);
    String accessToken = Jwts.builder()
						.setSubject(authentication.getName())
            .claim(AUTHORITIES_KEY, authorities)
            .setExpiration(accessTokenExpiresIn)
            .signWith(key, SignatureAlgorithm.HS256)
            .compact();

    String refreshToken = Jwts.builder()
            .setExpiration(new Date(now + REFRESH_TOKEN_EXPIRE_TIME))
            .signWith(key, SignatureAlgorithm.HS256)
            .compact();

    return TokenResponse.builder()
            .grantType(BEARER_TYPE)
            .accessToken(accessToken)
            .accessTokenExpiresIn(accessTokenExpiresIn.getTime())
            .refreshToken(refreshToken)
            .build();
}
```

✔️ **액세스 토큰을 생성하는 코드 부분**

```java
String accessToken = Jwts.builder()
				.setSubject(authentication.getName())
        .claim(AUTHORITIES_KEY, authorities)
        .setExpiration(accessTokenExpiresIn)
        .signWith(key, SignatureAlgorithm.HS256)
        .compact();
```

1. `Jwts.builder()` : JWT 토큰 생성을 위한 빌더 객체를 생성
2. `.setSubject(authentication.getName())` : 토큰의 subject(제목)을 인증 객체의 이름으로 설정
3. `.claim(AUTHORITIES_KEY, authorities)` : 토큰의 페이로드에 클레임 정보를 추가합니다. AUTHORITIES_KEY는 토큰에서 사용될 권한 정보를 저장하는 키 값이며, authorities는 인증 객체에서 가져온 권한 정보를 저장한다.
4. `.setExpiration(accessTokenExpiresIn)` : 토큰의 만료 시간을 설정
5. `.signWith(key, SignatureAlgorithm.HS256)` : 토큰의 서명을 설정
6. `.compact()` : 최종적으로 JWT 토큰을 생성하고 문자열 형태로 반환한다.

## 📌 Authentication

Spring Security은 사용자가 로그인하기 위해 자격 증명을 제공하면

- `Authentication Manager`가 이를 검증하고 **Authentication 객체를 생성한다.**
- Authentication에는 사용자의 인증 정보와 권한 정보가 포함됩니다.
- 이후 애플리케이션에서는 `SecurityContextHolder`를 통해 Authentication 객체에 액세스하여 사용자 권한을 확인하고, 이에 따라 액세스를 허용하거나 거부할 수 있다.
- Authentication는 `Principal` 객체와 `Credentials` 객체, 그리고 `Authorities`객체로 구성된다.

## 👉🏻 Principal

```java
public interface Principal {
    String getName();
}
```

- Principal 객체는 인증된 사용자의 정보를 포함하며 사용자의 아이디나 이메일과 같은 식별자를 저장한다.
- Principal 객체는 **사용자를 구별**하는 유일한 정보를 제공하며, Authentication 객체를 통해 애플리케이션 전반에서 이를 사용하여 사용자를 식별한다.
- Spring Security에서 Principal 객체는 일반적으로 인터페이스를 구현한 `UserDetails` 객체를 사용하여 인증된 사용자의 정보를 제공한다.

## 👉🏻 Credentials

- 인증에 사용된 **자격 증명을 포함**하며 일반적으로 패스워드와 같은 사용자의 암호화된 비밀번호를 저장한다. → 이 정보는 인증과정에 사용되며, **인증된 사용자가 자신의 자격 증명 정보를 확인할 때 사용한다.**

### ❓ Principal객체와 Credentials 객체의 차이**
    
    두 객체 모두 스프링 시큐리티에서 인증에 사용되는 객체이지만, 각각 다른 정보를 담고 있다. 
    
    `Credentials`
    
    - 사용자의 **자격 증명 정보**를 담고 있음 (패스워드, 토큰, 인증서 등)
    - 인증된 사용자가 자신의 자격 증명 정보를 확인할 때 사용
    
    `Principal 객체` 
    
    - 인증된 사용자의 **식별정보**를 담음 (이메일, 아이디)
    - 인증된 사용자를 식별하는데 사용하며, 인증된 사용자가 애플리케이션에 수행할 작업에 대한 권한등을 결정하는데 사용
    

## 👉🏻 Authorities

인증된 사용자가 가진 권한 정보를 포함합니다. 권한 정보는 일반적으로 역할(role) 또는 권한(permission)으로 나타냅니다.

## 📌 UserDetails

UserDetails는 **인증된 사용자의 정보를 담고 있는 인터페이스이다.** UserDetails는 인증에 필요한 사용자 정보를 제공하는 메서드를 정의하고 있으며, 사용자의 아이디, 비밀번호, 권한 정보 등을 포함하고 있다.

```java
public interface UserDetails extends Serializable {
    Collection<? extends GrantedAuthority> getAuthorities();

    String getPassword();

    String getUsername();

    boolean isAccountNonExpired();

    boolean isAccountNonLocked();

    boolean isCredentialsNonExpired();

    boolean isEnabled();
}
```

- `getAuthorities()` : 인증된 사용자의 권한 정보를 반환합니다.
- `getPassword()` : 인증된 사용자의 비밀번호를 반환합니다.
- `getUsername()` : 인증된 사용자의 아이디를 반환합니다.
- `isAccountNonExpired()` : 인증된 사용자의 계정이 만료되었는지 여부를 반환합니다.
- `isAccountNonLocked()` : 인증된 사용자의 계정이 잠겨 있는지 여부를 반환합니다.
- `isCredentialsNonExpired()` : 인증된 사용자의 자격 증명이 만료되었는지 여부를 반환합니다.
- `isEnabled()` : 인증된 사용자의 계정이 활성화되어 있는지 여부를 반환합니다.

### ❓Principal 객체와 UserDetails의 차이가 무엇일까?**
    
    
   Principal 객체와 UserDetails 객체는 인증된 사용자의 정보를 제공하는 데 사용되는 객체라는 공통점을 지닌다. 하지만 **Principal 객체는 인증된 사용자의 아이디 정보만을 제공**하는 반면, **UserDetails 객체는 인증된 사용자의 모든 정보를 제공**합니다.
   보통 Spring Security에서 인증이 완료되면, Authentication 객체가 생성되고,  Authentication 객체는 인증된 사용자의 정보를 참조하는 Principal 객체와 사용자의 모든 정보를 담고 있는 UserDetails 객체를 가지고 있습니다.
    즉, **Principal 객체는 UserDetails 객체의 일부 정보만을 제공하는 래퍼(Wrapper) 객체**이며, UserDetails 객체는 실제 인증된 사용자의 모든 정보를 담고 있는 객체입니다. 
    Authentication 객체는 Principal 객체와 UserDetails 객체를 함께 가지고 있으며, 인증된 사용자의 정보를 참조하기 위해 이 두 객체를 활용합니다.
    따라서 Spring Security에서 Principal 객체와 UserDetails 객체는 밀접한 관계를 가지고 있으며, 두 객체 모두 인증된 사용자의 정보를 제공하는 데 사용됩니다.
