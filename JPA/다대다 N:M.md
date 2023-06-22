# 다대다 N:M

<img width="497" alt="스크린샷 2023-06-22 오후 3 39 23" src="https://github.com/yaezzin/TIL/assets/97823928/32c47eb1-b3cf-485b-9493-ac2319e8eb6b">

관계형 데이터베이스는 테이블 2개로 다대다 관계를 표현할 수 없다. 따라서 일대다, 다대일 관계로 풀어내는 연결테이블을 사용한다.
* '회원들은 상품을 주문한다. 반대로 상품들은 회원들에 의해 주문된다.' 이러한 관계는 회원 테이블과 상품 테이블만으로는 표현할 수 없다.
* 따라서 Member+Product 연결 테이블을 추가하여 다대다 관계를 풀어내었다.


<img width="500" alt="스크린샷 2023-06-22 오후 3 40 54" src="https://github.com/yaezzin/TIL/assets/97823928/df334611-d5ee-4077-ab52-19f3ca7d0e36">

반면 객체는 테이블과 다르게 객체 2개로 다대다 관계를 만들 수 있다.
* 회원 객체는 컬렉션을 사용해서 상품들을 참조하면 되고, 반대로 상품들도 컬렉션을 사용해서 회원들을 참조하면 된다.
* 따라서 @ManyToMany를 사용하면 다대다 관계를 편리하게 매핑할 수 있다.

## 다대다 단방향

```java
@Entity
public class Member {

    @Id
    @Column(name = "MEMBER_ID")
    private Long id;

    @ManyToMany
    @JoinTable(name = "member_product,
    		  joinColumns = @JoinColumn(name = "MEMBER_ID"),
              inverseJoinColumn = @JoinColumn(name = "PRODUCT_ID"))
    private List<Product> products = new ArrayList<>();
    
    @Column(name = "USERNAME")
    private String username;
}

@Entity
public class Product {
	
    @Id
    @Column(name = "PRODUCT_ID")
    private Long id;
    
    @Column(name = "NAME")
    private String name;
}
```
* ManyToMany와 @JoinTable을 통해 연결 테이블을 바로 매핑해주었다. 따라서 회원_상품 엔티티 없이 매핑을 완료할 수 있다.
* ```@JoinTable(name = )``` : 연결 테이블을 지정한다.
* ```@JoinTable(joinColumns = )``` : 현재 방향인 회원과 매핑할 조인 컬럼 정보를 지정한다
* ```@JoinTable(inverseJoinColumn = )``` : 반대 방향인 상품과 매핑할 조인 컬럼 정보를 지정한다.


## 다대다 양방향

```java
@Entity
public class Member {

    @Id
    @Column(name = "MEMBER_ID")
    private Long id;

    @ManyToMany
    @JoinTable(name = "member_product,
    		  joinColumns = @JoinColumn(name = "MEMBER_ID"),
              inverseJoinColumn = @JoinColumn(name = "PRODUCT_ID"))
    private List<Product> products = new ArrayList<>();
    
    @Column(name = "USERNAME")
    private String username;

}

@Entity
public class Product {
	
    @Id
    @Column(name = "PRODUCT_ID")
    private Long id;
    
    @Column(name = "NAME")
    private String name;

    @ManyToMany(mappedBy = "products")
    private List<Member> members;
}
```

## 다대다 문제점

@ManyToMany를 사용하면 연결 테이블을 자도으로 처리해주므로 도메인 모델이 단순해지고 간편해진다.
* 하지만 실무에서는 잘 사용하지 않는데, 예를 들어 회원이 상품을 주문하면 연결 테이블에 단순히 주문한 회원아이디와 상품 아이디만 담고 끝나지 않는다.
* 즉, 주문 수량 컬럼이나 주문한 날짜 같은 컬럼을 더 담게 된다. 이렇게 컬럼을 추가하면 더는 @ManyToMany를 더이상 사용할 수 없다.

## @IdClass 사용 - 식별관계

```java
@Entity
public class Member {

    @Id
    @Column(name = "MEMBER_ID")
    private Long id;

    @OneToMany(mappedBy = "member")
    private List<Product> products = new ArrayList<>();
    
}

@Entity
public class Product {
	
    @Id
    @Column(name = "PRODUCT_ID")
    private Long id;
    
    @Column(name = "NAME")
    private String name;
}

@Entity
@IdClass(MemberProductId.class)
public class MemberProduct {
    @Id
    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;

    @Id
    @ManyToOne
    @JoinColumn(name = "PRODUCT_ID")
    private Product product;

   // 새로운 컬럼 추가
   private int orderAmount;

}

public class MemberProductId implements Serializable {
    private String member;
    private String product;

   ...
}
```
* 상품 엔티티를 보면 상품 엔티티에서 회원상품 엔티티로 객체 그래프 탐색 기능이 필요하지 않다고 판단하여 연관관계를 만들지 않았다.
* 회원 상품 엔티티는 기본키가 Member_id, Product_id로 이루어진 복합기본키다.
  * @IdClass를 사용해서 식별자 클래스를 지정 -> MemberProductId 클래스를 복합 키를 위한 식별자 클래스로 사용한다.
  * 🔥 회원상품(MemberProduct)는 회원의 기본 키를 받아서 자신의 기본키로 사용함과 동시에, 회원과의 관계를 위한 외래키로 사용한다. - ```식별관계```

#### +) 복합키를 위한 식별자클래스의 특징

* 복합 키는 별도의 식별자 클래스를 만들어야 한다.
* Serializable을 구현해야 한다.
* equals, hashCode 메소드를 구현해야한다.
* 기본 생성자가 있어야 한다.
* 식별자 클래스는 public이여야 한다.

#### 저장

```java
Member member = new Member();
em.persist(member);

Product product = new Product();
em.persist(product);

MemberPorduct mp = new MemberProduct();
mp.setMember(member);
mp.setProduct(product);
em.persist(mp);
```
* 회원 상품 엔티티를 만들면서 연관된 회원 엔티티와 상품 엔티티를 설정한다.
* 회원 상품 에티티는 데이터베이스에 저장될 때 연관된 회원의 식별자와 상품의 식별자를 가져와서 자신의 기본 키 값으로 사용한다.

#### 조회

```java
MemberProductId mpi = new MemberProductId();
mpi.setMember("member1");
mpi.setProduct("product1");

MemberProduct mp = em.find(MemberProduct.class, mpi);

Member member = memberProduct.getMember();
Product product = memberProduct.getProduct();
```
* 복합키는 항상 식별자 클래스를 만들어야하고, em.find()를 보면 생성한 식별자 클래스로 엔티티를 조회한다.
* 복합키를 사용하게 되면 상대적으로 ORM 매핑에서 처리할 일이 많아진다.

## 새로운 기본 키 사용 - 비식별 관계

추천하는 방법은 데이터베이스에서 자동으로 생성해주는 대리 키를 Long값으로 사용하는 것이다.
* 이는 ORM 매핑시에 복합키를 만들지 않아도 되어 간단히 매핑을 완성할 수 있다.

```java
@Entity
@Table(name = "ORDER")
public class Order {

   @Id
   @GeneratedValue
   @Column(name = "ORDER_ID")
   private Long id;

   @ManyToOne
   @JoinColumn(name = "MEMBER_ID")
   private Member member;

   @ManyToOne
   @JoinColumn(name = "PRODUCT_ID")
   private Product product;

   @Column(name = "ORDER_AMOUNT")
   private int orderAmount;

}
```
* order_id라는 기본키를 만들고, member_id, product_id는 외래키로만 사용한다.
  * 즉, 받아온 식별자는 외래키로 사용하고 새로운 식별자를 추가하는 방법 - ```비식별 관계``` 
* 대리키를 사용하면 식별관계에 복합키를 사용하는 것보다 매핑이 단순하고 이해하기 쉽다.
 

#### 저장

```java
Member member = new Member();
em.persist(member);

Product product = new Product();
em.persist(product);

Order order = new Order();
order.setMember(member); // 연관관계 설정
order.setProduct(product); // 연관관계 설정
order.setOrderAmount(2);
em.persist(order);
```

#### 조회

```java
Long orderId = 1L;
Order order = em.find(Order.class, orderId);

Member member = order.getMember();
...
```

* 식별자 클래스를 사용하지 않아 코드가 한결 더 단순해지고, 객체 입장에서도 더 편리하게 ORM 매핑을 할 수 있다.
