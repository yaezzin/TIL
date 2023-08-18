# Docker

## 가상머신 vs 도커 

### 가상 머신

> 하이퍼바이저(Hypervisor)를 이용해 여러 개의 운영체제를 하나의 호스트에서 생성해서 사용하는 방식

<img width="340" alt="스크린샷 2023-08-18 오후 5 14 36" src="https://github.com/yaezzin/TIL/assets/97823928/b78a217a-18b1-4ec6-9cee-c8ebd9cb293f">

* 각 가상머신에는 우분투, CentOS 등의 운영체제가 설치되어 사용됨
* 각 게스트 운영체제는 **다른 게스트 운영체제와는 완전히 독립된 공간과 시스템 자원을 할당받아 사용**하며, 이러한 작업은 **하이퍼바이저를 거치기 때문에** 성능 저하 발생!
* 각 가상머신은 자체 운영체제, 라이브러리, 애플리케이션 등을 가져 격리가 강화되지만, 높은 시스템 리소스 사용량과 무겁고 느린 시작 속도가 문제가 됨

### 도커 컨테이너

<img width="340" alt="스크린샷 2023-08-18 오후 5 44 01" src="https://github.com/yaezzin/TIL/assets/97823928/c1c0adb0-3edc-41b7-a2b6-5c13cf1594fc">

* 도커 컨테이너는 가상화된 공간을 생성할 때 리눅스 자체 기능을 사용하여 프로세스 단위의 격리 환경을 만드므로 성능 저하가 거의 없음
* 가상 머신과 달리 호스트 운영체제가 **커널을 공유하면서 격리된 환경을 제공**하기 때문에 컨테이너는 라이브러리와 실행파일만 있으면 됨

### 도커 구성요소

<img width="500" alt="스크린샷 2023-08-18 오후 5 50 18" src="https://github.com/yaezzin/TIL/assets/97823928/48c932dc-778f-495f-9d8d-401c22ce66f0">

* ```Docker Client``` : 도커를 설치하면 그것이 Client며 build, pull, run 등의 도커 명령어를 수행
* ```DOCKER_HOST``` : 도커가 띄워져있는 서버를로 DOCKER_HOST에서 컨테이너와 이미지를 관리
* ```Docker daemon``` : 도커 엔진
* ```Registry``` : 외부(remote) 이미지 저장소로, 다른 사람들이 공유한 이미지를 내부(local) 도커 호스트에 pull할 수 있음 -> 이렇게 가져온 이미지를 run하면 컨테이너가 됨

## 이미지와 컨테이너

<img width="609" alt="스크린샷 2023-08-18 오후 5 52 26" src="https://github.com/yaezzin/TIL/assets/97823928/5fecea6d-5dd8-4b06-9f4b-80ed9f5acf4e">

### 이미지란?

### 컨테이너란?

### 이미지 설치법
<img width="600" alt="스크린샷 2023-08-18 오후 4 39 01" src="https://github.com/yaezzin/TIL/assets/97823928/e2a40201-48dd-4b3d-8083-9973a22dbfc4">

```shell
# 이미지 설치
docker pull {NAME}

# 이미지 설치 확인
docker images
```
* Docker Hub - Explore 에서 docker의 이미지를 검색할 수 있음
* 예를 들어 ```docker pull httpd``` 라는 명령어로 우리컴퓨터의 docker에 httpd 도커이미지를 설치 할 수 있음

### 컨테이너 실행

* run 명령어를 통해 이미지를 실행시켜서 컨테이너를 만듦


## 네트워크 설정

## 볼륨

## 참고

* https://seosh817.tistory.com/345
