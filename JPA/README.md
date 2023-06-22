# JPA

* [JPA란?](https://github.com/yaezzin/TIL/blob/main/JPA/JPA%EB%9E%80.md)
  * [JPA와 Hibernate의 차이점](https://github.com/yaezzin/TIL/blob/main/JPA/JPA%EB%9E%80.md)
  * [JPA와 Spring Data JPA의 차이점](https://github.com/yaezzin/TIL/blob/main/JPA/JPA%EB%9E%80.md)
  * [JPA를 사용하는 이유](https://github.com/yaezzin/TIL/blob/main/JPA/JPA%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%20%EC%9D%B4%EC%9C%A0.md)

* [영속성 컨텍스트]()
  * [엔티티 매니저 팩토리와 엔티티 매니저](https://github.com/yaezzin/TIL/blob/main/JPA/Entity%20Manager.md) 
  * [영속성 컨텍스트와 생명주기](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%98%81%EC%86%8D%EC%84%B1%20%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EC%99%80%20%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0.md)
  * [영속성 컨텍스트의 장점](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%98%81%EC%86%8D%EC%84%B1%20%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EC%9D%98%20%EC%9D%B4%EC%A0%90.md)
  * [1차 캐시](https://github.com/yaezzin/TIL/blob/main/JPA/1%EC%B0%A8%20%EC%BA%90%EC%8B%9C.md)
  * [2차 캐시](https://github.com/yaezzin/TIL/blob/main/JPA/2%EC%B0%A8%EC%BA%90%EC%8B%9C.md)
  * [플러시 - DB 동기화](https://github.com/yaezzin/TIL/blob/main/JPA/%ED%94%8C%EB%9F%AC%EC%8B%9C%20flush.md)
 
* [지연로딩과 즉시로딩](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%A6%89%EC%8B%9C%EB%A1%9C%EB%94%A9%EA%B3%BC%20%EC%A7%80%EC%97%B0%EB%A1%9C%EB%94%A9.md)
  * [프록시 (Proxy)와 지연로딩](https://github.com/yaezzin/TIL/blob/main/JPA/%ED%94%84%EB%A1%9D%EC%8B%9C%EC%99%80%20%EC%A7%80%EC%97%B0%EB%A1%9C%EB%94%A9.md) 
  * [영속성 전이 CASCADE, 고아객체](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%98%81%EC%86%8D%EC%84%B1%20%EC%A0%84%EC%9D%B4.md)

* [기본 키 생성 전략](https://github.com/yaezzin/TIL/blob/main/JPA/%EA%B8%B0%EB%B3%B8%ED%82%A4%20%EC%83%9D%EC%84%B1%20%EC%A0%84%EB%9E%B5%EC%97%90%20%EB%94%B0%EB%A5%B8%20%EB%8F%99%EC%9E%91%20%EB%B0%A9%EC%8B%9D%20%EC%B0%A8%EC%9D%B4.md)

* [연관 관계 매핑]()
  * [일대일 1:1 - ```@OneToOne```](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%9D%BC%EB%8C%80%EC%9D%BC%201%3A1.md)
  * [일대일 1:1 양방향 지연로딩 이슈](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%9D%BC%EB%8C%80%EC%9D%BC%20%EC%96%91%EB%B0%A9%ED%96%A5%20%EB%A7%A4%ED%95%91%20%EC%8B%9C%20%EC%A7%80%EC%97%B0%EB%A1%9C%EB%94%A9%20%EC%9D%B4%EC%8A%88.md)
  * [일대다 1:N - ```@OneToMany```](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%9D%BC%EB%8C%80%EB%8B%A4%201%3AN.md)
  * [일대다 1:N 양방향 매핑 지양 이유](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%9D%BC%EB%8C%80%EB%8B%A4%20%EB%8B%A8%EB%B0%A9%ED%96%A5%20%EB%A7%A4%ED%95%91%EC%9D%84%20%EC%A7%80%EC%96%91%ED%95%98%EB%8A%94%20%EC%9D%B4%EC%9C%A0.md)
  * [다대일 N:1 - ```@ManyToOne```](https://github.com/yaezzin/TIL/blob/main/JPA/%EB%8B%A4%EB%8C%80%EC%9D%BC%20N%3A1.md)
  * [다대다 N:M - ```@ManyToMany```](https://github.com/yaezzin/TIL/blob/main/JPA/%EB%8B%A4%EB%8C%80%EB%8B%A4%20N%3AM.md)
  * [연관 관계 주인](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%97%B0%EA%B4%80%20%EA%B4%80%EA%B3%84%20%EC%A3%BC%EC%9D%B8.md)

* [ETC]()
  * [상속 관계 매핑](https://github.com/yaezzin/TIL/blob/main/JPA/%EC%83%81%EC%86%8D%20%EA%B4%80%EA%B3%84%20%EB%A7%A4%ED%95%91.md)
  * [@MappedSuperclass](https://github.com/yaezzin/TIL/blob/main/JPA/%40MappedSuperclass.md)
  * [복합키와 식별 관계 매핑]()
  * [@IdClass, @EmbeddedId 어떤 것이 더 적합할까?](https://github.com/yaezzin/TIL/blob/main/JPA/%EB%B3%B5%ED%95%A9%ED%82%A4%20%40IdClass%20%40EmbeddedId.md)
  * [조인 테이블]()

* [트랜잭션 관리]()
  * [@Transactional 애너테이션]()
  * [트랜잭션 전파 옵션]()

* [페이징 처리 방법]()

* [쿼리]()
  * [Criteria API와 Querydsl 중 어떤 것을 선택하는 것이 좋을까?]()
  * [JPA에서 쿼리 성능 최적화 방법]()
  * [JPA에서의 쿼리 최적화]()
