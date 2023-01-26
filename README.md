# Today-I-Learned 

명확하게 모르는 개념들을 정리하고 기록하는 공간

- [x] [Java](#-java) 
- [x] [Spring](#-spring)
- [x] [OS](#-operating-system)
- [x] [Database](#-database)
- [x] [Network](#-network)

## 💡 Java

### Basic
* [클래스와 객체](https://github.com/yaezzin/TIL/issues/37)
* [JVM의 구조와 동작방식](https://github.com/yaezzin/TIL/issues/21)
* [가비지 컬렉션 (Garbage Collection)](https://github.com/yaezzin/TIL/issues/31)
* [Call by Reference와 Call by Value의 차이]()
* [원시타입(Primitive Type) vs 레퍼런스 타입(Reference Type)]()
* [Static vs Non-Static](https://github.com/yaezzin/TIL/issues/22#issue-1332037927)
  * [Static과 메모리 적재 위치](https://github.com/yaezzin/TIL/issues/22#issuecomment-1208295619)
  * [Java의 main 메서드가 static인 이유](https://github.com/yaezzin/TIL/issues/22#issuecomment-1333531727)
* [this()와 ](https://github.com/yaezzin/TIL/issues/27#issue-1341381062) [this](https://github.com/yaezzin/TIL/issues/27#issuecomment-1217698249)

### OOP

* [접근 제어자](https://github.com/yaezzin/TIL/issues/34#issue-1479117147)
* [오버로딩 vs 오버라이딩](https://github.com/yaezzin/TIL/issues/38#issue-1526826170)
* [다형성 (polymorphism)](https://github.com/yaezzin/TIL/issues/35#issue-1484253040)
  * [업캐스팅(Up-casting), 다운캐스팅(Down-casting)](https://github.com/yaezzin/TIL/issues/35#issuecomment-1342434822)
  * [istanceof 연산자](https://github.com/yaezzin/TIL/issues/35#issuecomment-1342435480)
* [추상클래스 vs 인터페이스](https://github.com/yaezzin/TIL/issues/2)  

### Exception Handling

* [예외처리(Exception Handling)](https://github.com/yaezzin/TIL/issues/39#issue-1534331369)
  * [try-catch-finally](https://github.com/yaezzin/TIL/issues/39#issuecomment-1383527100)
  * [try-with-resources](https://github.com/yaezzin/TIL/issues/39#issuecomment-1383573343)
  * [User Defined Exception](https://github.com/yaezzin/TIL/issues/39#issuecomment-1383585031)
* [printStackTrace()을 지양하는 이유](https://github.com/yaezzin/TIL/issues/39#issuecomment-1383546386)
* [Check Exception와 Unchecked Exception]()
* [throw와 throws 차이](https://github.com/yaezzin/TIL/issues/39#issuecomment-1383566793)

### java.lang

* [Object 클래스의 clone() 메서드]()
  * [깊은 복사(Deep Copy) vs 얕은 복사(Shallow Copy)]()
* [String, StringBuilder, StringBuffer]()  
* [래퍼(wrapper)클래스]() 

### Collection FrameWork
 
* [```List```](https://github.com/yaezzin/TIL/issues/40#issue-1536023208)
  * [ArrayList, LinkedList, Vector](https://github.com/yaezzin/TIL/issues/40#issuecomment-1385118862)
  * [ArrayList와 LinkedList 성능 차이](https://github.com/yaezzin/TIL/issues/40#issuecomment-1386646821)
* [```Set```](https://github.com/yaezzin/TIL/issues/40#issue-1536023208)
  * [HashSet](https://github.com/yaezzin/TIL/issues/43) 
  * [TreeSet](https://github.com/yaezzin/TIL/issues/44)
* [```Map```](https://github.com/yaezzin/TIL/issues/40#issue-1536023208)
  * [HashMap]()
  * [LinkedHashMap]()
  * [TreeMap]()
  * [HashMap과 HashTable]()  
* [Iterable과 Iterator](https://github.com/yaezzin/TIL/issues/41#issue-1548608308)
* [Comparator와 Comparable](https://github.com/yaezzin/TIL/issues/42)

### Generics

* [지네릭스(Generics)](https://github.com/yaezzin/TIL/issues/45)
* [와일드 카드]()


## 💡 Spring

### Spring
* [Spring vs SpringBoot]()
* [AOP (Aspect Oriented Programming )]()
* [의존성 주입(DI)과 제어의 역전(IoC)]()

### JPA
* [JPA를 사용하지 않았을 때의 문제점들](https://github.com/yaezzin/TIL/issues/13)
* [영속성 컨텍스트와 생명주기](https://github.com/yaezzin/TIL/issues/14)
* [영속성 컨텍스트의 이점](https://github.com/yaezzin/TIL/issues/15)
* [기본 키 생성 전략에 따른 동작 방식 차이](https://github.com/yaezzin/TIL/issues/16)
* [일대다 단방향 매핑을 지양하는 이유](https://github.com/yaezzin/TIL/issues/17)
* [프록시와 지연로딩](https://github.com/yaezzin/TIL/issues/20)
* [@Transactional 사용 이유](https://github.com/yaezzin/TIL/issues/1)
* [복합키 @IdClass와 @EmbeddedId 선택](https://github.com/yaezzin/TIL/issues/19)
* [JPA N+1 문제]()

### JDBC
* [Spring Data JDBC 공식문서 정복하기 🌱](https://github.com/yaezzin/TIL/issues/32)
* [JDBC와 JPA]()
* [JDBC Template 기록하기](https://minutemaid.tistory.com/177?category=1256443)
* [JDBC, SQLMAPPER, ORM](https://github.com/yaezzin/TIL/issues/36)


### Security & Jwt
* [Spring Security를 이용한 Jwt 적용]()

## 💡 Database
* [인덱스 (Index)](https://github.com/yaezzin/backend-notes/issues/28)
* [정규화 (Normalization)](https://github.com/yaezzin/TIL/issues/29)
* [트랜잭션 (Transaction)](https://github.com/yaezzin/TIL/issues/30)
* [식별관계와 비식별 관계](https://github.com/yaezzin/backend-notes/issues/18)

## 💡 Operating System
* [운영체제 총정리]()

## 💡 Network
* [네트워크 총 정리](https://github.com/yaezzin/TIL/issues/33)
* [HTTP (HyperText Transfer Protocol)](https://github.com/yaezzin/TIL/issues/25)
* [패킷의 구조](https://github.com/yaezzin/TIL/issues/24)
