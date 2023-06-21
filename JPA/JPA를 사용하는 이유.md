# JPA를 사용하는 이유?

## 1. SQL 의존적인 개발 X

* 요구 사항이 변경되면서 필드가 추가/삭제될 때 관련된 SQL의 DAO를 열어서 모두 수정해주어야 하는 문제 발생
* 따라서 개발자들이 DAO를 열어서 어떤 SQL이 실행되고, 어떤 객체들이 함께 조회되는지 일일이 확인해야함
* 이는 물리적으로 SQL이나 JDBC API를 데이터 접근 계층에 숨기는데 성공했을 지라도, 논리적으로는 엔티티와 강한 의존 관계를 가짐

## 2. 객체 관계 매핑

```
1. 회원 조회용 SQL 작성
select member_id, name from member m where member_id = ?

2. JDBC API를 통해 SQL 실행
ResultSet rs = stmt.executeQuery(sql);

3. 조회 결과를 Member 객체로 매핑
String memberId = rs.getString("MEMBER_ID");
String name = rs.getString("NAME");

Member member = new Member();
member.setMemberId(memberId);
member.setName(name);
```

예를들어 MemberDAO의 find()메서드를 통해 회원을 조회하는 기능을 개발하게 되면,
* 개발자가 객체지향 애플리케이션과 데이터베이스 중간에서 SQL과 JDBC API를 사용해서 변환작업을 직접 해주어야함

```java
public Member find(Long memberId) {
    Member member = em.find(Member.class, memberId);
    return member;
}
```

하지만 JPA를 사용하게 되면 
* JPA는 내부적으로 SQL을 생성하고 JDBC API를 사용하여 데이터베이스에서 데이터를 가져와 Member 객체로 매핑하므로
* 개발자는 SQL 변환 작업을 직접 처리하지 않아도 되는 장점을 가짐

## 3. 객체 그래프 탐색

```java
Member member = entityManager.find(Member.class, memberId); 
List<Order> orders = member.getOrders();
```

JPA를 사용하면 find() 메서드를 호출하여 Member 객체를 가져올 때, 
* JPA가 적절한 SQL을 생성하여 데이터베이스에서 회원 객체를 가져옴
* JPA가 연관된 주문 객체들을 자동으로 로드하여 회원 객체와 연결
* 따라서 getOrders() 메서드를 호출하면 연관된 주문 객체들을 즉시 사용할 수 있음

하지만 JPA를 사용하지 않게 되면
* 직접 SQL과 JDBC API를 사용하여 Member 객체를 조회하기 위해 SQL을 작성하고 실행한 후,
* 결과를 기반으로 Member 객체를 생성하고 필드에 값을 설정하고,
* 이후 연관된 주문(Order) 객체들을 조회하기 위해 추가적인 SQL을 작성하고 실행하여 주문 객체들을 생성하고 필드에 값을 설정해야 함

```결론``` : JPA를 사용하지 않으면 객체를 중심으로 코드를 작성하는 것이 아니라 SQL과 데이터베이스와의 변환 작업을 직접 처리해야 하므로 더 많은 코드 작성과 번거로움이 발생!!

## 4. 동일성 비교

JPA를 사용하지 않으면 데이터베이스의 같은 로우를 조회하더라도, 객체 측면에서는 둘은 다르 인스턴스 이기 때문에 동일성 비교(==) 시에 false가 반한됨  

하지만 JPA는 **같은 트랜잭션일 때 같은 객체가 조회되는 것을 보장**
* 따라서 같은 트랜잭션 내에서 동일한 엔티티를 여러 번 조회하면, 영속성 컨텍스트는 이미 메모리에 ```캐싱```된 엔티티를 반환하므로 데이터베이스를 매번 접근할 필요가 X
* 또한 JPA의 ```변경감지```는 같은 트랜잭션 내에서 엔티티의 변경 사항을 추적하므로, 엔티티를 조회한 후 값이 변경되었을 때 알아차릴 수 있게 되어 데이터베이스에 효율적으로 반영할 수 있게 됨

