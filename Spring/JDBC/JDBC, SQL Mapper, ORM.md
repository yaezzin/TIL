# JDBC, SQL Mapper, ORM

```
들어가기 전..
우아한테크코스 10분 테코톡 유튜브 영상들을 보고 정리한 내용들입니다. 
- [코즈의 JDBC, SQL Mapper, ORM] : https://www.youtube.com/watch?v=mezbxKGu68Y
```

<img width="405" alt="스크린샷 2022-12-12 오후 7 52 50" src="https://user-images.githubusercontent.com/97823928/207027506-2198693c-afb7-4588-bc6b-2861cc29b0c0.png">

## 세 기술의 공통점?

### 영속성 (Persistence)
* 데이터를 생성한 프로그램의 실행이 종료되더라도 사라지지 않는 데이터의 특성
* 영구히 저장되는 것

세 기술 모두 어떻게 DB에 저장할 것인가, 어떻게 영속성을 구현할 것인가가 공통적인 관심사이다!


## JDBC API

> 자바 진영의 데이터베이스 연결 표준 인터페이스

<img width="400" alt="스크린샷 2022-12-12 오후 7 56 23" src="https://user-images.githubusercontent.com/97823928/207028200-147e9d84-40cf-4000-89c0-d7d1ced93f35.png">

* ```등장배경``` : 1990년대 중반 인터넷 보급, DB 산업 성장, 온라인 비즈니스의 투자 증가로 DB 커넥터에 대한 니즈 증가

* 구조도를 보면 우리가 사용하는 API를 변경하지 않고도 **JDBC Driver Manager만 갈아끼워주면** 어떤 제품이든 접근할 수 있도록 해주는 것이 JDBC API이다.
  * 1 ) Driver Manager를 통해 DB와 APP을 연결하여 커넥션 인스턴스를 얻는다.
  * 2 ) 커넥션을 통해서 Statement를 얻는다.
  * 3 ) Statement를 통해 ResultSet을 얻는다. 
* ```불편한 점``` : 
  * 중복 코드가 많다.  EX) 연결 생성, 명령문, ResultSet 닫기, 연결 등
  * 쿼리를 일일이 써야한다.
  * 커넥션 관리를 계속 해주어야한다.
  * 데이터베이스 로직에서 예외 처리 코드를 수행해야 한다. 등등 ...


## SQLMAPPER - JDBC, MyBatis

### Spring JDBC 

SQL Mapper는 객체의 필드와 SQL문을 매핑하여 데이터를 객체화한다.  
여기서 객체와 관계를 매핑하는 것이 아니라 직접 작성한 SQL문의 질의 결과와 객체의 필드를 매핑하여 데이터를 객체화 하는 것이다.

<img width="520" alt="스크린샷 2022-12-12 오후 8 06 53" src="https://user-images.githubusercontent.com/97823928/207030205-fa9737df-c436-4957-9388-8bcc8f742ac9.png">

Spring JDBC의 경우 자바의 JDBC의 불편함을 해소하기 위해서 JDBC 인터페이스에서 ```DataSource```를 통해 커넥션을 위한 설정들을 관리하도록 해준다.

#### Spring JDBC가 하는 일
* 자원의 생성과 반환(Connection, Statement, ResultSet 등)
* Statement 실행
* ResultSet Loop 처리
* Exception 처리와 반환
* Transaction 처리

#### Spring JDBC에서 개발자가 할 일
* 핵심적으로 해야될 작업만 해주면 나머지는 Framwork가 알아서 처리해준다.
* datasource 설정
* sql문 작성
* 결과 처리

### MyBatis

* ```등장배경``` : 자바 코드에서 String으로 SQL을 작성하면 에러가 많이 발생한다. 그러므로 자바 코드와 SQL를 분리하는 것이 목표
* 쿼리를 자바코드에서 ```XML 파일```에서 관리하게 됨 -> 코드의 복잡성 감소
* 개발자는 Mapper Interface와 Mapping File만 구현하면 됨
* 하지만 여전히 SQL에 의존적임 
 
</br>

<img width="430" alt="스크린샷 2022-12-12 오후 8 29 24" src="https://user-images.githubusercontent.com/97823928/207034393-f8ffd656-9a5c-4e64-874f-837fdf765f2f.png">

#### 동작과정
* 애플리케이션 시작시 SqlSessionFactoryBuilder가 Configuration 파일을 읽어서 SqlSessionFactory를 생성
* 이후 Application단에서 데이터 접근 작업시 SqlSessionFactory는 매 요청마다 SqlSession 객체를 생성
* SqlSession 객체를 통해 DB 작업을 진행하는데, 작업시 수행하는 쿼리는 mapper 파일에 담겨있음

## ORM - Hibernate, Spring Data JDBC, Spring Data JPA

객체지향적으로 구현된 모델과 관계형 DB와 연결하는 것이 어려워서 등장한 것이 ORM

### IF 객체의 필드가 추가된다면
* SQL을 모두 수정할 필요가 없음 
* 직접 쿼리를 작성하는 경우 쿼리 변경이 필수적 
  * ex ) insert into User(userIdx, name, password) values(?, ?, ?)에서 필드 추가가 필수적임

#### 패러다임의 불일치

관계형 데이터베이스(데이터 중심의 구조)와 객체지향(추상화, 상속, 다형성 .. )의 경우 설계원칙이 너무 다르기 때문에 관계형 데이터베이스에서는 연관관계나 상속 등을 표현하기 어려움

#### 목표 : 객체와 테이블을 매핑해서 데이터를 객체화 하자!

* 객체관의 관계를 기반으로 SQL을 자동으로 생성하고 메서드를 통해 조작함
  * ```select * from User U where U.userIdx = ?```를
  * userReposiotory.findById() 처럼 사용하면 된당..
  * 필드가 추가되거나 삭제되면 쿼리의 수정 없이 해당 엔티티만 수정해주면 된다.

## 참고자료
* https://codedragon.tistory.com/5960
* https://github.com/WeareSoft/tech-interview/blob/master/contents/db.md#jdbc%EB%9E%80


