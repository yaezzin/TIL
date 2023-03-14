# Today-I-Learned 

명확하게 모르는 개념들을 모두 정리하고 기록중

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
* [🌱 Spring vs SpringBoot]()
* [AOP (Aspect Oriented Programming)]()
* [의존성 주입(DI)과 제어의 역전(IoC)]()
* [🔒 Spring Security 정리해보기](https://hungry-tithonia-878.notion.site/Spring-Security-8b986cb0a61c415bbe3807c37d73d22e)
    * [TokenProvider, Authentication, ...](https://hungry-tithonia-878.notion.site/TokenProvider-ae0f738f7fd3406ca55aaca95b85d858)
    * [UserDetailsService](https://hungry-tithonia-878.notion.site/UserDetailService-c83c40495aa34199aa0af7847c0d5a77)
    * [OncePerRequestFilter + SecurityContextHolder](https://hungry-tithonia-878.notion.site/OncePerRequestFilter-SecurityContextHolder-d774a5d5827f4f2b966c2727d60a297f)
    * [AuthenticationEntryPoint](https://hungry-tithonia-878.notion.site/AuthenticationEntryPoint-401-8869ffcae9ea440681c57ad1d486c694)
    * [CustomAccessDeniedHandler](https://hungry-tithonia-878.notion.site/CustomAccessDeniedHandler-403-fbfff73033a147338a3a9ef5e6a7d8a8)
    * [Principal 객체와 UserDetails 객체의 차이?](https://hungry-tithonia-878.notion.site/Principal-UserDetails-c9a07c42d28f4b3dab5e14f05a1314c8)

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

### MSA

* [MSA 아키텍쳐 패턴 🌥](https://hungry-tithonia-878.notion.site/c3ef66c3fa3c471b9b668ec67922cf85)

## 💡 Database
* [인덱스 (Index)](https://github.com/yaezzin/backend-notes/issues/28)
* [정규화 (Normalization)](https://github.com/yaezzin/TIL/issues/29)
* [트랜잭션 (Transaction)](https://github.com/yaezzin/TIL/issues/30)
* [식별관계와 비식별 관계](https://github.com/yaezzin/backend-notes/issues/18)

## 💡 Operating System

## 💡 Network
* [네트워크 총 정리](https://hungry-tithonia-878.notion.site/9c7bb9cb416d4c44b0393404ff831caa?v=f13a5e21e94543c3856d6c6939f031bb)
* [프로토콜](https://hungry-tithonia-878.notion.site/2c1d65c4d40d43769067cdf96fae23a6)
* [OSI 7계층](https://hungry-tithonia-878.notion.site/OSI-7-cd026180eabd4981b45f550392f41842)
  * [CRC 기법](https://hungry-tithonia-878.notion.site/CRC-ad64db3e4795470bb1a6909092ad1ba5)
* [흐름제어]()
  * [Stop-and-Wait](https://hungry-tithonia-878.notion.site/Stop-and-Wait-3c5c61b049c249a59a3b02ad63de89d6)
  * [Sliding Window](https://hungry-tithonia-878.notion.site/Sliding-Window-eb91d2d322fc4715bffc6ed6d116379f)
  * [MMS & MTU]()
* [네트워크 연결 구분 LAN, MAN, WAN](https://hungry-tithonia-878.notion.site/LAN-MAN-WAN-ed8a66cc5025475d8e69f0fb5fc591e4)
* [네트워크 통신 방식](https://hungry-tithonia-878.notion.site/7c8a29f4626a45f5b42c2b6a6f4c1aa2)
* [MAC 주소](https://hungry-tithonia-878.notion.site/MAC-9054efd00671427d8bdeb038500b74bd)
* [IP 주소](https://hungry-tithonia-878.notion.site/IP-af1700a898944625910c2dbf31945752)
  * [IPsec](https://hungry-tithonia-878.notion.site/IPsec-33aada9a081b4cf7a91a6477e8b844b7)
* [TCP &](https://hungry-tithonia-878.notion.site/TCP-e15b6fb7b72842c492d4ee8dc86378bd)[  UDP & ](https://hungry-tithonia-878.notion.site/UDP-8c54ecf665b2460fab6ccfc506a032fd)[ARP]()
* [HTTP (HyperText Transfer Protocol)](https://github.com/yaezzin/TIL/issues/25)
* [패킷의 구조](https://github.com/yaezzin/TIL/issues/24)
