>https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#jdbc.domain-driven-design

## 왜 Spring Data JDBC를 사용하는가?

```JPA```
* 엔티티의 변화를 추적함
* 지연로딩 지원 -> 동일한 범위의 DB설계에 객체 매핑이 가능

```JDBC```
* 엔티티를 로드하는 시점에 SQL문이 실행됨 
* 실행이 완료되면 엔티티가 완전히 로딩됨 (즉, 지연로딩이 작동하지 않음)
* 상태변경을 추적하거나 세션이 없음
* JPA의 경우 값을 수정하면 따로 update, 저장 쿼리를 날릴 필요 X
* 하지만 JDBC는 수정하고 저장하지 않으면 변경되지 않음

## 도메인 주도 설계와 관계형 데이터베이스

```Aggregate```
* 일관성이 보장되는 엔티티 그룹 (ex. Order, OrderItems ... )
  * 각 Aggregate는 항상 하나의 aggregate root를 가진다
  * aggregate는 aggregate root에 있는 메서드로만 조작되어야 한다 (atomic changes)
  * aggregate root 당 일반적으로 하나의 repository를 갖는다.
  * aggregate root에서 접근가능한 모든 엔티티는 한 aggregate에 속한다.
  * root가 아닌 entity를 저장하는 테이블에 대한 외래키는 aggregate만 가지고 있다.
* Aggregate간의 참조는 항상 일관성을 보장하지 않으나 결과적으로는 일관성(eventual consistent)을 보장한다.
* 현재로썬 root로부터 참조되는 엔티티는 Spring Data JDBC에 의해 삭제되고 다시 생성된다.

## Repository

```java
// Ex. Spring Data JDBC repositories using Java configuration
@Configuration
@EnableJdbcRepositories                                                                
class ApplicationConfig extends AbstractJdbcConfiguration {                            

    @Bean
    DataSource dataSource() {                                                         

        EmbeddedDatabaseBuilder builder = new EmbeddedDatabaseBuilder();
        return builder.setType(EmbeddedDatabaseType.HSQL).build();
    }

    @Bean
    NamedParameterJdbcOperations namedParameterJdbcOperations(DataSource dataSource) { 
        return new NamedParameterJdbcTemplate(dataSource);
    }

    @Bean
    TransactionManager transactionManager(DataSource dataSource) {                     
        return new DataSourceTransactionManager(dataSource);
    }
}
```
* ```@EnabledJdbcRepositories```는 Repository로부터 파생된 인터페이스로 구현
* AbstractJdbcConfiguation을 상속받아 JDBC에 필요한 다양한 디폴트 빈들을 사용할 수 있음
* public DataSource dataSource()는 데이터베이스에 연결하는 DataSource를 생성

### 작동 원리
* DataSource는 NamedParameterJdbcOperations와 TransactionManger를  셋업한다.
* 그후 @EnableJdbcRepositories를 통해 레포지토리를 활성화한다.
* 만약 base package가 없으면 configuartion 클래스가 있는 패키지를 사용한다.

## 영속 엔티티
aggregate는 ```CrudRepository.save(...)```를 통해 저장한다.
* 새로운 aggregate인 경우 root에 대한 insert가 이루어지고 직간접적으로 참조되는 엔티티에 대한 insert가 이루어진다.
* aggregate root가 새로운 것이 아닌 경우 참조된 모든 엔티티가 삭제되고 다시 삽입된다.
* Spring Data JDBC는 aggregate의 이전 상태를 알 수 없기 때문에 비효율적이다.

## 객체 매핑

리플렉션의 오버헤드를 피하기 위해 Spring Data 객체 생성에서는 기본적으로 런타임에 생성된 팩토리 클래스를 사용한다.
이 클래스는 도메인 클래스 생성자를 직접 호출합니다.
```java
class Person {   
    Person(String firstname, String lastname) {
          … 
    } 
}

class PersonObjectInstantiator implements ObjectInstantiator {    
    Object newInstance(Object... args) {     
        return new Person((String) args[0], (String) args[1]);   
    } 
}
```
이러한 방식으로 객체를 생성하면 10% 정도 성능을 개선할 수 있다.
하지만 다음과 같은 제약조건을 준수해야한다.
* private 클래스 X
*  non-static inner 클래스 X
* CGLib proxy 클래스 X
* Spring Data에 의해 사용될 생성자는 private이면 안됨

이러한 제약조건을 지키지 않는 경우 Spring Data는 reflection을 총해 entity를 초기화한다.
