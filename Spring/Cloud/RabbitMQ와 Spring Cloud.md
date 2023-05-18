
## RabiitMQ와 Spring Cloud

### 동작 흐름

* 1 ) 중앙 집중화된 구성 서버인 Spring Cloud Config Server(ex. config-service)에서 구성 정보가 변경되면, 변경 이벤트가 발생
* 2 ) 변경 이벤트는 RabbitMQ에게 전달 (구성 서버와 RabbitMQ와의 연결을 위해 Spring AMQP를 사용)
* 3 ) RabbitMQ는 변경 이벤트를 수신하고, 해당 이벤트를 구독 중인 Spring Cloud 애플리케이션(ex. memeber-service)에게 전달
* 4 ) 구독 중인 애플리케이션은 RabbitMQ로부터 이벤트를 받아 처리할 수 있는 메시지를 수신
* 5 ) Spring Cloud 애플리케이션은 RabbitMQ로부터 수신한 메시지를 처리하여 구성 정보를 업데이트

### 구현

#### 1. dependency

```java
implementation 'org.springframework.boot:spring-boot-starter-actuator'
implementation 'org.springframework.cloud:spring-cloud-starter-bus-amqp'
```

#### 2. rabbitmq 정보 추가

```java
spring:
  application:
    name: apigateway-service 
    
  # 각각의 마이크로 서비스에도 rabbitmq 정보를 다 추가
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest

# busrefresh 추가
management:
  endpoints:
    web:
      exposure:
        include: health, busrefresh
```

### 3. bootstrap.yml

config-service에서는 어디에서 구성정보를 가져올건지 설정해둔다. git, native 등의 방법을 사용할 수 있음.


```java
spring:
  cloud:
    config:
      server:
        native:
          search-locations: file://${user.home}/Desktop/study/native-file-repo
        git:
          uri: https://github.com/yaezzin/spring-cloud-config.git
```

각 마이크로 서비스에서는 config-service로 부터 ecommerce.yml의 정보들을 가져오게됨
```java
spring:
  cloud:
    config:
      uri: http://127.0.0.1:8888
      name: ecommerce
```

#### 4. 결과?

일단 native 파일 시스템 방식을 사용했고, 그 경로 안에있는 ecommerce.yml에서 정보를 사용하게 된다.
* 이후, ecommerce.yml값을 수정하고 localhost:8888/ecommerce/default에 접속하게 되면 변경된 정보를 확인할 수 있다.
* 하지만 아직은 다른 마이크로 서비스에 전달되지 않은 상태!
* /member-service/actuator/busrefresh 를 호출하게 되면 구성 정보가 업데이트 됨
  * actuator/refresh의 경우에는 각 마이크로 서비스에서 모두 호출해야지 구성정보가 업데이트 됨
  * 하지만 /actuator/busrefresh를 호출하면 member-service 뿐만 아니라 다른 마이크로 서비스에도 구성정보가 업데이트 됨!

