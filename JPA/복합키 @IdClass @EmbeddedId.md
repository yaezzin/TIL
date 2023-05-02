김영한님 JPA 책에서 복합키 매핑 파트를 읽다가 의문점이 남았다.  
복합키를 매핑하는 방법은 ```@IdClass```, ```@EmbeddedId``` 두 가지인데, 어떤 것이 더 적합한 어노테이션일까..?하고 고민해보았다.

참고한 자료는 다음과 같으며, 다른 사람들의 생각을 기반으로 내 생각과 함께 정리해보았다.
* [Composite Primary Keys in JPA](https://www.baeldung.com/jpa-composite-primary-keys)
* [StackOverFlow - Which annotation shoud I use IdClass or EmbeddedId](https://stackoverflow.com/questions/212350/which-annotation-should-i-use-idclass-or-embeddedid)
* [The Ultimate Guide on Composite IDs in JPA Entities](https://www.jpa-buddy.com/blog/the-ultimate-guide-on-composite-ids-in-jpa-entities/)
* [김영한님 자바 ORM 표준 JPA 프로그래밍](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788960777330)

## 복합 키

둘 이상의 컬럼으로 구성된 복합 기본 키는 다음과 같이 매핑할 수 없다. 

```java
@Entity
public class Hello {
    @Id
    private String id1;

    @Id
    private String id2; 
}
```

JPA에서 식별자를 둘 이상 사용하려면 **별도의 식별자 클래스**를 만들어야 한다. 
* 식별자 필드가 하나일 때는 보통 자바의 기본 타입을 사용하므로 문제가 없지만, 2개 이상인 경우 별도의 식별자 클래스를 만들고 그곳에 ```equals```와 ```hashCode```를 구현해야한다.
* JPA는 복합 키를 지원하기 위해 ```@IdClass```와 ```@EmbeddedId```를 지원하는데, 전자는 관계형 DB에 가까운 방법이며 후자는 좀 더 객체지향에 가까운 방법이다. (by 김영한님)

## @IdClass

```java
@Entity
@IdClass(AccountId.class)
public class Account {
    @Id
    private String accountNumber;

    @Id
    private String accountType;

    // ...
}

public class AccountId implements Serializable {

    private String accountNumber; // Account.accountNumber과 매핑
    private String accountType;  // Account.accountType과 매핑

    public AccountId() {
    }

    public AccountId(String accountNumber, String accountType) {
        this.accountNumber = accountNumber;
        this.accountType = accountType;
    }

    // equals() and hashCode() ...
}
```
@IdClass 방식의 경우 각각의 기본 키 컬럼을 @Id로 매핑하였다. 그 후 @IdClass를 사용하여 AcountId 클래스를 식별자 클래스로 지정해 주었다.

### 저장 방식

```java
Account account = new Account();
account.setAccountNumber("accountId1");
account.setAccountType("accountId2");
em.persist(account); 
```
* 저장할 때는 식별자 클래스인 AccountId가 보이지 않는데, em.persist()를 호출하면 영속성 컨텍스트에 엔티티를 저장하기 직전에 내부에서 Account.accountNumber, Account.accountType 값을 사용해서 식별자 클래스인 AccountId를 생성하고 이것을 영속성 컨텍스트의 키로 사용한다.

### 복합키로 조회 

```java
AccountId accountId = new AccountId("accountId1", "accountId2");
Account account = em.find(Account.class, "accountId");
```
* 조회의 경우 식별자 클래스인 AccountId로 엔티티를 조회한다.

## @EmbeddedId

```java
@Entity
public class Book {
    @EmbeddedId
    private BookId bookId;

    // ...
}

@Embeddable
public class BookId implements Serializable {
    
    @Column(name = "BOOK_ID1")
    private String title;
    @Column(name = "BOOK_ID2")
    private String language;

    // default constructor

    public BookId(String title, String language) {
        this.title = title;
        this.language = language;
    }

    // getters, equals() and hashCode() methods
}
```
@EmbbedId는 Book 엔티티에서 식별자 클래스 BookId를 직접 사용하고 @EmbeddedId를 붙어주면 된다.

### 저장

```java
Book book = new Book();
BookId bookId = new BookId("title1", "language1");
book.setId(bookId);
em.persist(book);
```
* IdClass와 다른 점은 저장할 때 식별자 클래스 bookId를 직접 생성해서 사용한다는 점이다.

### 조회

```java
BookId bookId = new BookId("title1", "language1");
Book book = em.find(Book.class, bookId);
```

## 그렇다면 어떤 것을 사용해야 할까?

### 1. JPQL 쿼리 관점
#### @IdClass

```sql
SELECT account.accountNumber FROM Account account
```
* 쿼리가 좀 더 단순하다.

#### @EmbeddedId

```sql
SELECT book.bookId.title FROM Book book
```
* Embedded를 사용하면 ```엔티티.식별자클래스.id```처럼 한번 더 식별자 클래스를 명시해주어야 한다.

### 2. Repository 관점 (?)

#### @EmbeddedId
```java
Book findByBookId(BookId bookId); 
```
* 상대적으로 더 간결해보임. 단순히 식별자 클래스를 이용해서 엔티티를 반환

#### @IdClass
```java
List<Book> findAllByBookId_title(String title); 
List<Book> findAllByBookId_language(String language); 
```
* 언더바(_)를 사용해서 구분해주어야 하며, 더 복잡해보임

### 3. sql 관점

#### @IdClass

```sql
select book.book_id1, book.book_id2,
from book
inner join books on book.book_id1=books.id1  
left outer join types on books.book_id2=books.book_id2  
where book.book_id1=? and book.book_id2=? 
```

#### @Embedded
```sql
select book_id1, book_id2
from book
where book_id1=? and book_id2=? 
```




## 결론

복합 키의 일부에 개별적으로 액세스 하는 경우는 @IdClass 를 사용하면 좋을 것 같으나, 식별자를 오브젝트로 자주 사용하는 장소에서는, @EmbeddedId 를 사용하는 것이 좋아보인다. 
그래서 김영한님 교재에서 @EmbeddedId가 더 객체지향적인 방식이라고 설명하신 것은 아닐까 싶다 (내 생각.. ㅎ,ㅎ)
