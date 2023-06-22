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

## 다대일 일대다

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
  * 회원상품(MemberProduct)는 회원의 기본 키를 받아서 자신의 기본키로 사용함과 동시에, 회원과의 관계를 위한 외래키로 사용한다. (상품도 동일하다.)

#### +) 복합키를 위한 식별자클래스의 특징

* 복합 키는 별도의 식별자 클래스를 만들어야 한다.
* Serializable을 구현해야 한다.
* equals, hashCode 메소드를 구현해야한다.
* 기본 생성자가 있어야 한다.
* 식별자 클래스는 public이여야 한다.
