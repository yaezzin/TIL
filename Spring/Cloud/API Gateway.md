## API Gateway

> 각 마이크로서비스에 대한 단일 진입점

* 다양한 마이크로서비스들의 요청을 받아들이고, 필요한 경우 변환, 라우팅, 필터링, 인증, 인가, 로깅 등의 작업을 수행한 후, 클라이언트에게 전달

![image](https://github.com/yaezzin/TIL/assets/97823928/e70fd803-1caa-46f6-aeb8-a8e14ae0454c)

* ```요청 라우팅```
  * 클라이언트의 요청을 해당하는 백엔드 서비스로 라우팅 
  * 여러 개의 백엔드 서비스를 사용하는 경우에도 클라이언트는 단일 엔드포인트를 사용하여 API Gateway를 통해 요청을 보낼 수 있음
* ```프로토콜 변환```
  * 클라이언트와 백엔드 서비스 간에 사용되는 프로토콜을 변환
  * 예를 들어, 클라이언트가 HTTPS를 사용하고 있는 경우에도 백엔드 서비스는 HTTP를 사용하고 있는 경우에 API Gateway가 이를 변환해줄 수 있음
* ```인증 및 인가``` 
  * API Gateway는 클라이언트의 요청에 대한 인증 및 인가를 처리
  * 사용자 인증, API 키, OAuth, JWT 등 다양한 인증 방식을 지원
* ```로드 밸런싱```
  * 백엔드 서비스들 사이에서 요청을 분산하여 로드 밸런싱을 수행
  * 이를 통해 서비스의 가용성과 성능을 향상시킬 수 있음
* ```캐싱```
  * API Gateway는 자주 요청되는 데이터에 대한 응답을 캐싱하여 중복 요청을 처리하지 않고 이전에 받은 응답을 재사용할 수 있음. 
  * 이를 통해 응답 시간을 단축시킬 수 있음
* ```모니터링 및 로깅``` 
  * API Gateway는 요청 및 응답에 대한 로깅을 제공하고, 모니터링 및 분석 도구와 통합할 수 있음
  * 이를 통해 서비스의 성능 및 문제를 실시간으로 모니터링할 수 있음


## Netflix Ribbon

```Spring cloud에서 MSA간의 통신 방법```

### 1 ) RestTemplate

```java
RestTemplate restTemplate = new RestTemplate();
restTemplate.getForObject("http://localhost:8080/", User.class, 200);
```
* 요청 주소, 포트번호 등을 입력해서 api 호출

### 2 ) Feign Client

```java
@FeignClient("stores")
public interface StoreClient {
    @RequestMapping(method = RequestMethod.GET, value="/stores")
    List<Store> getStores();
}
```

* @Feign Client를 사용하면 단지 인터페이스를 정의하고 메서드에 원하는 HTTP 요청을 어노테이션으로 정의하면 됨
* 클라이언트는 서비스의 이름으로 통신할 수 있음
