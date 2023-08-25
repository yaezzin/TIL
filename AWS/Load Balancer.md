# 로드 밸런서

## Load Balancer

몰려있는 트래픽을 분산처리하여 서버에 부하가 집중되지 않도록 하는 서비스
* 트래픽을 하나의 경로로 받아서 다수의 인스턴스에 분산하게 되어, 유저 입장에서는 각각의 인스턴스에 일일히 접근해서 관리하지 않고 하나의 주소로 접속해서 관리할 수 있게 됨

## ELB (Elastic Load Balancer)

<img width="450" alt="스크린샷 2023-08-25 오후 1 37 33" src="https://github.com/yaezzin/TIL/assets/97823928/500bb3fc-7715-4441-8190-9ba34ad821b4">

* ELB는 VPC에 탑재되며, 사용자의 요청을 받고 VPC 내의 EC2에 부하를 분산함
* 외부의 요청을 받아들이는 ```리스너```와 요청을 분산할 리소스의 집합인 ```타겟 그룹```으로 구성
* 리스너는 프로토콜과 포트를 기반으로 요청을 받아 검사하고, 적절한 타겟으로 전달
  * 하나의 인스턴스당 하나 이상의 포트로 트래픽을 전달할 수 없으며, IP 주소로 전달할 수 없음
  * 즉 명시적으로 지정된 EC2 인스턴스나 ECS 또는 EKS의 컨테이너로만 전달할 수 있음
* 타겟 그룹은 해당 그룹에 등록된 EC2의 각종 정보가 적혀있고, 이 EC2가 전달받은 요청을 처리할 수 있는지를 체크하는 헬스체크 기능, 요청 처리가 가능한 EC2가 가능한지 등을 확인하는 모니터링 기능 등을 가짐

### ELB 요청 처리 과정

<img width="484" alt="스크린샷 2023-08-25 오후 1 41 21" src="https://github.com/yaezzin/TIL/assets/97823928/0fac1e02-5bfc-47f9-a0bf-d202a8c252b2">

1. 사용자가 로드밸런서에 접근하기 위해 DNS 서버에 로드밸런서의 도메인 해셕을 요청  
2. DNS 서버가 로드 밸런서 노드 IP리스트를 사용자에게 전달  
3. 사용자는 전달받은 IP 중 하나를 선택하여 로드밸런서에 접근  
4. 사용자는 포트가 일치하는 리스너에 접근하게 되고, 리스너가 이 요청에 적절한 타겟 그룹으로 전달  
5. EC2가 요청을 처리한 후 사용자에게 반환  

### ELB 서비스의 종류

- Application Load Balancer(ALB)
- Network Load Balancer(NLB)
- Classic Load Balancer(CLB)
- Gateway Load Balancer(GLB)

## ALB (Application Load Balancer)

<img width="507" alt="스크린샷 2023-08-25 오후 1 54 24" src="https://github.com/yaezzin/TIL/assets/97823928/77fcf973-8f3c-4c58-abeb-8dae9a0e6f56">

```Layer 7```
* HTTP/HTTPS의 헤더 정보를 이용하여 웹 애플리케이션 서비스에 걸리는 부하를 분산해주는 로드 밸런서
* ALB는 로드 밸런서 하나만 설정하면 트래픽을 모니터링 하여 각 타겟그룹에 라우팅한다.
* /user 경로로 오면 람다 대상 그룹으로 보내고, /shop으로 오면 회원관리 대상그룹에 보내주는 식으로!
* 또한 포트에 따라 다른 타겟그룹으로 요청을 라우팅할 수 있으며, 람다 함수로도 요청을 라우팅할 수 있음
* SSL 적용이 가능하여 타겟 그룹의 EC2를 대신하여 SSL 암호화/복호화를 대신 진행할 수 있음

## NLB

<img width="505" alt="스크린샷 2023-08-25 오전 11 32 53" src="https://github.com/yaezzin/TIL/assets/97823928/a4edd6ab-d983-418d-82e4-30250ffdf6a8">

```Layer 4```
* TCP/UDP를 사용하는 요청을 받아들여 부하분산을 수행
* 데이터 안을 들여다보지 않고 패킷 레벨에서만 부하분산을 하기 떄문에 속도가 빠르고 효율이 높음 (but 섬세한 라우팅 불가능)
* HTTP가 아닌 TCP층에서 처리하기 때문에 HTTP 헤더를 해석하지 못함 
* NLB는 SSL 적용이 인프라단에서 불가능하여 애플리케이션에서 따로 적용해야 함
* 고성능을 요구하는 환경에서의 부하분산에 적합
* NLB는 공인 IP를 고정할 수 있다 (ALB는 X)

## ALB vs NLB

```ALB```
* 클라이언트가 웹화면을 요청하는 상황일때 (HTTP,HTTPS 프로토콜을 사용해서 어플리케이션 레벨 접근할때

```NLB```
* 내부로 들어온 트래픽을 처리하고, 내부의 인스턴스로 트래픽을 전송할때

## 참고

* https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-ELB-Elastic-Load-Balancer-%EA%B0%9C%EB%85%90-%EC%9B%90%EB%A6%AC-%EA%B5%AC%EC%B6%95-%EC%84%B8%ED%8C%85-CLB-ALB-NLB-GLB
* https://velog.io/@yange/%EB%82%B4%EB%B6%80%EB%A7%9D%ED%8F%90%EC%87%84%EB%A7%9D%EC%97%90%EC%84%9C-repository-%EA%B5%AC%EC%84%B1
