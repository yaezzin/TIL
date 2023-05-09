## 1. Entity Manager Factory

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
```

* JPA에서 엔티티 매니저를 생성하는 팩토리
* 엔티티 매니저 팩토리는 하나의 애플리케이션에서 **한 번만** 생성되며, 데이터베이스와의 연결을 관리
* Persistence 클래스의 createEntityManagerFactory() 메서드를 사용하여 생성할 수 있음

## 2. Entity Manager

```java
EntityManager em = emf.createEntityManager();
```

* 영속성 컨텍스트를 관리하고, 데이터베이스와 상호작용하는 객체
* 엔티티를 데이터베이스에 저장하거나 조회하는 등의 작업을 수행
* 엔티티 매니저 팩토리로부터 생성되며, 애플리케이션에서 필요할 때마다 생성하여 사용
* 엔티티 매니저는 EntityManagerFactory 클래스의 createEntityManager() 메서드를 사용하여 생성할 수 있음
* 엔티티 매니저는 **데이터베이스 연결이 꼭 필요한 시점까지 커넥션을 얻지 않음** (예를 들어, 트랜잭션을 시작할 떄 커넥션을 획득)
