# JPA를 사용하지 않았을 때의 문제점들

JPA를 사용하지 않았을 때의 여러 상황들을 살펴보고, JPA가 어떠한 문제점을 해결하는 지 알아보자!  

## 1. SQL을 직접 다루어야 하는 문제점

보통 자바 애플리케이션은 ```JDBC API```를 통하여 ```SQL```을 ```데이터베이스```에 전달한다.  
* 하지만 데이터베이스는 객체와는 다른 구조를 가지기 때문에 객체를 직접적으로 데이터베이스에 저장하거나 조회할 수 없다.  
* 따라서 개발자가 객체지향 애플리케이션과 데이터베이스 사이에서 SQL과 JDBC API를 사용해서 변환작업을 해주어야 한다. 
* 또한 애플리케이션은 테이블들에 대한 CRUD가 만들어지는데, 각 테이블에 **필드가 추가/삭제 될 때마다** 관련 쿼리들을 찾아서 모두 변경해주어야 한다. 

#### 즉, JPA를 사용하지 않으면 ❓
1. 만약 테이블이 100개라면 무수히 많은 SQL을 작성해야하고, 테이블마다 비슷한 작업을 수없이 반복해야한다.
2. 필드가 추가/삭제될 때마다 DAO를 열어서 어떤 SQL이 실행되고, 어떤 객체들이 함께 조회해야하는지 일일이 확인해야한다.

#### JPA를 사용하면 ❓
* 개발자가 직접 SQL을 작성하는 것이 아닌 JPA가 제공하는 API를 사용하면 된다. JPA가 적절한 SQL을 생성하여 데이터베이스에 전달한다!
* JPA는 연관된 객체를 사용하는 시점에 적절한 SELECT SQL을 실행한다.
* 즉 실제 객체를 사용하는 시점까지 데이터베이스 조회를 미룬다 (= 지연 로딩)


## 2. 상속관계에서의 문제점

#### 2-1 ) 저장
JDBC API를 사용해서 상속관계에 있는 객체에서 자식 객체를 저장하려면 부모, 자식 객체를 분해해서 SQL을 만들어야 한다.

```sql
INSERT INTO PARENT
INSERT INTO CHILD
```

* 즉, 부모 객체에서 부모 데이터만 꺼내서 INSERT SQL을 작성하고, 자식 객체에서 자식 데이터만 꺼내서 INSERT SQL을 작성해야 한다.
* 또한 자식 타입에 따라서 DTYPE도 저장해야 하므로 작성해야 할 코드량이 매우 많다.

#### 2-2 ) 조회

만약 자식을 조회한다면 부모와 자식 테이블을 조인해서 조회한 다음 그 결과로 자식 객체를 생성해야 한다.

### 하지만 JPA를 사용하면
* 개발자는 자바 컬렉션에 객체를 저장하듯이 객체를 저장하면 된다. ```jpa.persist(child)```
* JPA가 부모와 자식 객체를 분해하여 SQL을 실행하고 두 테이블에 나누어 객체를 저장한다.
* 또한, 자식 객체를 조회할 때도 find() 메소드를 통하여 저장하면된다. ```Child child = jpa.find(Child.class, childId);```
* 그러면 JPA가 부모와 자식 테이블을 조인해서 필요한 데이터를 조회하고 결과를 반환해준다.

## 3. 연관관계에서의 문제점

객체는 ```참조```를 통해서 연관된 객체를 조회하는 반면 테이블은 ```외래키```를 사용하여 조인을 사용하여 연관된 테이블을 조회한다.
* 또한 객체는 참조가 있는 방향으로만 조회가 가능하다. 
* ( member →  team 연관관계라면 ) member.getTeam()은 가능하나 team.getMember()는 불가능하다.
* 하지만 테이블은 외래 키 하나로 MEMBER JOIN TEAM, TEAM JOIN MEMBER 모두 가능하다.

이처럼 참조를 통한 객체지향 모델링을 사용하면 객체를 테이블에 저장하거나 조회하기 쉽지않다.
* Member 객체는 team 필드 (Team team 참조)를 토앟여 연관관계를 맺고, MEMBER 테이블은 TEAM_ID 외래키로 연관관계를 맺기 때문이다.
* 객체모델은 외래 키가 필요 없고 참조만 있으면 되는 반면, 테이블은 참조가 필요 없고 외래 키만 있으면 된다.
* 이는, 개발자가 중간에서 변환 작업을 해주어야 한다는 것이다.

### 3-1 ) 저장

객체를 데이터베이스에 저장하려면 team 필드(참조)를 TEAM_ID 외래 키 값으로 변환해야 한다.

```java
member.getId(); 
member.getTeam().getId(); # team 필드를 TEAM_ID 외래키로 변환하기 위해 외래키를 구함
member.getUsername();
```

### 3-2 ) 조회

객체를 조회할 때는 TEAM_ID 외래 키 값을 Member 객체의 team 참조로 변환해서 객체에 보관해야 한다.

```java
Member member = new Member();
Team team = new Team();
member.setTeam(team);  # 연관관게 설정
return member;
```

#### JPA를 사용한다면

```java
member.setTeam(team);
jpa.persist(member);
```
1. 개발자는 회원과 팀의 관계를 설정하고 회원 객체를 저장하면 된다.
2. JPA는 team의 참조를 외래 키로 변환하여 INSERT SQL을 데이터베이스에 전달한다.

```java
Member member = jpa.find(Member.class, memberId);
Team team = member.getTeam();
```
3. 조회 시에도 외래 키를 참조로 변환하는 일도 JPA가 처리해준다.

### 3-3 )  객체 그래프 탐색
<img width="576" alt="스크린샷 2023-01-27 오후 3 47 18" src="https://user-images.githubusercontent.com/97823928/215025904-3895ff8f-8177-4d17-8af9-69528a56f5ef.png">

객체 연관관계가 위와 같이 설계되어있다고 가정해보자.

```java
Member member = memberDAO.find(memberId);

/* find SQL*/
select m.*, t.* 
from Member m
join Team t on m.team_id = t.team_id
```
예를들어 MemberDAO에서 member 객체를 조회할 때 (= memberDAO.find)
회원과 팀에 대한 데이터만 조회했다면(= 회원과 팀만 조회하는 SQL이라면)
member.getTeam()은 성공하지만 member.getOrder() 같은 다른 객체 그래프는 데이터가 없으므로 탐색할 수 없다.

```java
memberDAO.getMember();
memberDAO.getMemberWithTeam();
memberDAO.getMemberWithOrderWithDelivery(); 
```

우리가 직접 SQL을 다루게 되면 
* 처음 실행하는 SQL에 따라 객체 그래프를 어디까지 탐색할 수 있는지 정해진다. 하지만 제약이 너무 크다!!
* 그러므로 위와 같이 MemberDAO에 회원을 조회하는 메소드를 상황에 따라 여러 개 만들어 사용해야 한다. (하지만 너무 복잡..)

### 하지만 JPA를 사용하면
연관된 객체를 사용하는 시점에 JPA 가 적절한 Select SQL을 실행하기 때문에 연관된 객체를 신뢰하고 마음껏 조회할 수 있다.
* 이 기능은 실제 객체를 사용하는 시점까지 데이터베이스 조회를 미룬다고 해서 ```지연로딩```이라고 한다.

```java
Member member = jpa.find(Member.class, memberId);
Order order= member.getOrder();
order.getOrderDate(); // Order을 사용하는 시점에서 select order sql
```
* 여기서 마지막줄에서 실제 Order을 사용하는 시점에 JPA는 데이터베이스에서 해당 테이블을 조회한다
* Member을 사용할 때마다 Order을 함께 사용하는 경우가 많다면 한 테이블씩 조회하는 것보다는 조인을 통해 함꼐 조회하는 것이 효과적이다.
* JPA는 연관된 객체를 즉시 함께 조회할지, 아니면 실제 사용할 시점에 조회할지를 간단한 설정으로 정의할 수 있다.

## 비교에서 문제점

```java
동일성 비교 : == , 객체 인스턴스의 주소 값을 비교한다.
동등성 비교 : equals(), 객체 내부의 값을 비교한다.
```
데이터베이스는 기본키의 값으로 각 로우를 구분하는 반면 객체는 동일성 비교와 동등성 비교라는 두가지 방법을 통해 구분한다.

```java
Member member1 = memberDAO.getMember(memberId);
Member member2 = memberDAO.getMember(memberId);
member1 == member2; // false
```
위의 코드를 보면 같은 기본키로 회원 객체를 2번 조회했다. 하지만 동일성비교(==)를 하면 false가 리턴된다.
* 같은 데이터베이스 로우에서 조회했음에도, 객체 측면에서는 둘인 다른 인스턴스이기 때문이다 (주소값이 서로 다름)
* 하지만 객체를 컬렉션에 저장했다면(list) 동일성 비교에 성공했을 것이다.

#### 하지만 JPA는 같은 트랜잭션일 떄 같은 객체가 조회하는 것을 보장한다.

```java
Member member1 = jpa.find(Member.class, memberId);
Member member2 = jpa.find(Member.class, memberId);
member1 == member2; // true
```
그러므로 다음 코드에서 두 멤버는 동일성 비교에 성공한다.

## 참고자료
* 김영한 [자바 ORM 표준 JPA 프로그래밍]을 기반으로 작성하였다.
