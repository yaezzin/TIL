# @Transactional

이전까지 @Transactional의 경우 단순히 CRUD 메서드 위에 적어주곤 했었고, 명확히 어떠한 쓰임을 가지는지에 대해 고민해본 적이 없었다.
그래서 이번 기회에 이 어노테이션의 쓰임과 사용이유 등에 대해서 기록하고자 한다.

## 1.  왜 사용하나요 ?
* 스프링의 서비스 계층에서 데이터베이스의 데이터를 변경하다 문제가 발생하는 경우 ```롤백```하기 위해 @Transactional 어노테이션을 사용
* 서비스 계층은 비즈니스 로직을 수행하므로 데이터베이스의 데이터를 변경하기 위해 DAO를 호출함 → 대부분 서비스 계층에 이 어노테이션을 사용

## 2. 모든 예외와 에러에 롤백하나요?

* 트랜잭션 어노테이션은 ```😵‍💫 모든 예외 및 에러에 대해 롤백 처리하지 않음```
* ```Runtime Exception``` 또는 ```Error```가 발생한 경우 롤백되며, Checked Exception에 대해서는 롤백되지 않음
* Checked Exception 예외도 롤백되게 하려면 트랜잭션 어노테이션에 Exception.class를 rollbackFor에 추가해야 함


|종류|설명|
|:----:|----|
|Runtime Exception| 개발자가 처리하기 어려운 예외로 프로그램 실행 중에 발생하는 예외를 의미 |
|Checked Exception| 프로그램이 제어할 수 없지만 개발자가 충분히 처리 가능한 예외|
|Error| ErrorException이 아닌 경우로 시스템 메모리 부족처럼 예측 및 처리가 어려움 |

## 3. Exception.class를 설정해주는 것이 좋은가요?

```@Transactional(rollbackFor = {Exception.class})```  

스프링은 왜 Checked Exception을 Default로 설정하지 않았을까? 
* Checked Excpetion은 개발자가 직접 처리할 수 있기 때문
* 서비스에서 Checked Excpetion이 발생했는데 롤백되지 않은 경우는 개발자가 예외 처리를 제대로 하지 않아서 발생한 문제로 간주함
  
## 4. @Transactional(readOnly = true)

Transactional 어노테이션의 속성 중에 readOnly 속성은 뭘까?
* 이 옵션을 주게 되면 스프링 프레임워크가 하이버 네이트 세션 플러시 모드를 ```MANUAL```로 설정
* 이렇게 되면 ```강제로 플러시를 호출하지 않는 한 플러시가 일어나지 않음```
* 따라서 커밋하더라도 영속성 컨텍스트가 플러시 되지 않기 때문에 엔티티의 등록, 수정, 삭제가 동작하지 않음
* 즉, 읽기 전용이기 떄문에 영속성 컨텍스트는 변경 감지를 위한 스냅샷을 보관하지 않으므로 성능이 향상됨

