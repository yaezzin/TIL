## @MappedSuperclass

부모 클래스는 테이블과 매핑하지 않고 부모 클래스를 상속받는 자식 클래스에게 매핑 정보만 제공하고싶으면 해당 어노테이션을 사용하면 된다.

```java
@MappedSuperclass
public abstract class BaseEntity {
    private LocalDateTime createdDate;
    private LocalDateTime lastModifiedDate;
}
```
* BaseEntity에는 객체들이 주로 사용하는 공통 매핑 정보를 정의한다.
* @Entity는 실제 테이블과 매핑되지만 @MappedSuperclass는 실제 테이블과 매핑되지 않고 단순히 매핑 정보를 상속하기만 한다.
* 이 클래스는 직접 생성해서 사용할 일이 거의 없으므로 추상클래스로 만드는 것이 권장된다.

```java
@Entity
public class Member extends BaseEntity{

    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    private String name;
}
```
* Member 엔티티는 상속을 통해 BaseEntity의 매핑 정보를 물려받는다.
* 만약 부모로부터 상속받은 매핑 정보를 재정의하려면 @AttributeOverride(하나 재정의)나  @AttributeOverrides 사용하면 된다.
  
