## JPA란?

<img width="436" alt="스크린샷 2023-06-21 오후 2 45 31" src="https://github.com/yaezzin/TIL/assets/97823928/2fa81d31-e660-4f00-bae8-80406e3cc7e9">
<br></br>

> 자바 진영에서 ```ORM 기술을 표준화한 인터페이스```
* ORM은 다양한 프레임워크와 라이브러리에서 구현되어 왔고, 각각의 구현체들은 고유한 방식과 기능을 가지고 있었는데,
  * 이때 ORM 구현체들이 각각의 방식으로 동작하여, 개발자들은 ORM을 사용할 때마다 해당 구현체에 맞는 API를 학습하고 사용해야 했음
  * JPA는 이러한 상황을 개선하기 위해 **ORM을 표준화한 인터페이스**를 제공하게 됨
* JPA는 자바 애플리케이션과 JDBC API사이에서 동작하며, 자바 인터페이스로 정의되어 있음

## ORM이란?

> 객체와 관계형 데이터베이스 간의 매핑을 지원하는 기술
* 객체와 관계형 데이터베이스를 매핑을 지원하여 개발자가 SQL 쿼리를 직접 작성하는 것보다 객체 지향적인 방식으로 데이터를 다룰 수 있게 함

## JPA와 Hibernate의 차이

> Hibernate는 ```JPA의 구현체 중 하나```
* JPA는 ORM 기술을 표준화한 인터페이스이고, Hibernate는 JPA를 구현한 구체적인 라이브러리

## JPA와 Spring Data JPA의 차이

<img width="400" alt="스크린샷 2023-06-21 오후 3 06 34" src="https://github.com/yaezzin/TIL/assets/97823928/7639b0c1-c7fc-48f9-aaf7-afc0a9ef90cb">

> Spring Data JPA는 JPA를 보다 편리하게 사용하기 위하 기능들을 제공
* 기존에는 EntityManager를 주입받아야 했지만, Spring Data JPA는 JPA를 한단계 더 추상화한 Repository 인터페이스를 제공
  * JPA를 추상화 했다는 말은, Spring Data JPA의 Repository의 구현에서 JPA를 사용한다는 것!
* 따라서 Spring Data JPA를 사용하면 사용자가 Repository 인터페이스를 정의만 하더라도 반복적인 CRUD를 줄일 수 있음
* 또한 동적 쿼리 생성, 페이징 처리 등을 지원함
