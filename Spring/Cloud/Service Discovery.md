## Service Discovery

서비스 디스커버리는 마이크로서비스 아키텍처에서 서비스들을 발견하고 통신하기 위한 메커니즘으로, 
각각의 마이크로서비스 인스턴스가 자신의 위치와 상태를 등록하고, 다른 서비스들이 이를 검색하고 호출할 수 있도록 돕는다.
* 즉, **각각의 마이크로서비스가 어느 위치에 있는지를 등록해 놓은 곳**
* 요청 정보가 Load Balancer(API Gateway)에 전달되면 다음으로 Service Discovery에 전달된다.
  * 이후 Service Discovery는 필요한 서비스가 어느 곳에 있는지에 대한 정보를 API Gateway로 반환하고 API Gateway는 이에 따라 해당 서비스를 호출하고 결과를 받게 된다.

## Eureka 

유레카는 Netflix에서 개발한 서비스 디스커리 서버로, 클라우드 환경에서 마이크로 서비스들의 등록, 검색, 로드 밸런싱 등을 관리하는 역할을 수행한다.
* 마이크로 서비스는 유레카 클라이언트를 사용해서 자신을 등록하고, 유레카 서버는 이 등록된 정보를 유지하고 관리하낟.

## 구현

#### build.gradle
```java
implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-server'
```

#### @EnableEurekaServer

```java
@SpringBootApplication
@EnableEurekaServer // 추가
public class DiscoveryserviceApplication {

	public static void main(String[] args) {
		SpringApplication.run(DiscoveryserviceApplication.class, args);
	}

}
```

#### application.yml
```yml
server:
  port: 8761

spring:
  application:
    name: discoveryservice
    
eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```
* ```register-with-eureka``` : 
  * Eureka 서버에 자동으로 등록할지 여부를 나타낸다. false로 설정되어 있으므로, 이 클라이언트는 Eureka 서버에 자동으로 등록되지 않는다.
  * 기본적으로 유레카 라이브러리가 포함된 채 서비스가 구동이되면, 유레카 클라이언트의 역할로서 어딘가에 등록하는 작업을 시도한다.
  * 하지만 현재 유레카 서버(= 클라이언트를 등록하고 관리하는 역할)을 만드는 중이므로 자신의 정보를 자신에게 등록할 필요가 없으므로 false로 설정한다.
* ```fetch-registry``` :
  * Eureka 서버의 서비스 목록을 로컬로 가져올지 여부를 나타낸다.
  * 마찬가지로, false로 설정되어 있으므로 이 클라이언트는 Eureka 서버로부터 서비스 목록을 가져오지 않는다.
* localhost:8761로 접속하면 유레카 서버가 구동된 것을 확인할 수 있다.
