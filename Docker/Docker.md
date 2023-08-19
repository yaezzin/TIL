# Docker

## 가상 머신

> 하이퍼바이저(Hypervisor)를 이용해 여러 개의 운영체제를 하나의 호스트에서 생성해서 사용하는 방식

<img width="340" alt="스크린샷 2023-08-18 오후 5 14 36" src="https://github.com/yaezzin/TIL/assets/97823928/b78a217a-18b1-4ec6-9cee-c8ebd9cb293f">

* 각 가상머신에는 우분투, CentOS 등의 운영체제가 설치되어 사용됨
* 각 게스트 운영체제는 **다른 게스트 운영체제와는 완전히 독립된 공간과 시스템 자원을 할당받아 사용**하며, 이러한 작업은 **하이퍼바이저를 거치기 때문에** 성능 저하 발생!
* 각 가상머신은 자체 운영체제, 라이브러리, 애플리케이션 등을 가져 격리가 강화되지만, 높은 시스템 리소스 사용량과 무겁고 느린 시작 속도가 문제가 됨

## 도커 컨테이너

<img width="340" alt="스크린샷 2023-08-18 오후 5 44 01" src="https://github.com/yaezzin/TIL/assets/97823928/c1c0adb0-3edc-41b7-a2b6-5c13cf1594fc">

* 도커 컨테이너는 가상화된 공간을 생성할 때 리눅스 자체 기능을 사용하여 프로세스 단위의 격리 환경을 만드므로 성능 저하가 거의 없음
* 가상 머신과 달리 호스트 운영체제가 **커널을 공유하면서 격리된 환경을 제공**하기 때문에 컨테이너는 라이브러리와 실행파일만 있으면 됨

## 도커 구성요소

<img width="500" alt="스크린샷 2023-08-18 오후 5 50 18" src="https://github.com/yaezzin/TIL/assets/97823928/48c932dc-778f-495f-9d8d-401c22ce66f0">

* ```Docker Client``` : 도커를 설치하면 그것이 Client며 build, pull, run 등의 도커 명령어를 수행
* ```DOCKER_HOST``` : 도커가 띄워져있는 서버를로 DOCKER_HOST에서 컨테이너와 이미지를 관리
* ```Docker daemon``` : 도커 엔진
* ```Registry``` : 외부(remote) 이미지 저장소로, 다른 사람들이 공유한 이미지를 내부(local) 도커 호스트에 pull할 수 있음 -> 이렇게 가져온 이미지를 run하면 컨테이너가 됨

## 이미지와 컨테이너

<img width="600" alt="스크린샷 2023-08-18 오후 5 52 26" src="https://github.com/yaezzin/TIL/assets/97823928/5fecea6d-5dd8-4b06-9f4b-80ed9f5acf4e">

```Dockerfile```
* 도커 이미지를 생성하기 위한 설정 파일
* docker build 명령어를 실행시키면 도커 이미지를 만들 수 있음

```Docker image```
* 애플리케이션과 애플리케이션 실행에 필요한 모든 것을 포함하는 파일 시스템과 설정으로 구성된 패키지
* 애플리케이션 코드, 라이브러리, 실행 환경, 설정 파일 등을 하나의 단위로 묶어서 관리

```Docker container```
* 도커 컨테이너는 도커 이미지의 인스턴스로 이미지를 기반으로 컨테이너가 생성되며, 컨테이너는 격리된 프로세스 공간, 파일 시스템, 네트워크를 가짐
* 도커 이미지를를 docker run 명령어를 실행시키면 컨테이너를 만들 수 있음

## 이미지 설치법

<img width="600" alt="스크린샷 2023-08-18 오후 4 39 01" src="https://github.com/yaezzin/TIL/assets/97823928/e2a40201-48dd-4b3d-8083-9973a22dbfc4">

```shell
# 이미지 설치
docker pull {NAME}

# 이미지 설치 확인
docker images
```

* Docker Hub - Explore 에서 docker의 이미지를 검색할 수 있음
* 예를 들어 ```docker pull httpd``` 라는 명령어로 우리컴퓨터의 docker에 httpd 도커이미지를 설치 할 수 있음

## 컨테이너 실행

```
# 실행
docker run {IMAGE NAME}

# 컨테이너 확인
docker ps
docker ps -a

# 컨테이너 종료 (삭제되는 것X)
docker stop {IMAGE NAME OR ID}

# 컨테이너 시작
docker start {IMAGE NAME OR ID}

#
docker logs

# 컨테이너 삭제
docker rm {IMAGE NAME OR ID}

# 이미지 삭제
docker rmi {IMAGE NAME}
```

## 네트워크 설정

<img width="600" alt="스크린샷 2023-08-19 오후 6 07 31" src="https://github.com/yaezzin/TIL/assets/97823928/a3cf7274-c389-482e-96f3-3c0ee3e0a8f1">

```
docker run -p {host port} : {container port} {image}

ex ) docker run -p 80:8000 httpd
```
* 웹브라우저에서 웹서버에 url로 요청을 하면
* 웹서버가 도커에서는 host라는 별도의 환경 안에 container로서 실행
* 따라서 host에서 받는 포트와 컨테이너 웹서버에서 받는 포트를 이어주어야 함
* 8080 포트로 진입하는 연결을 컨테이너안에서는 80포트로 이어주는 포트포워딩

<img width="317" alt="스크린샷 2023-08-19 오후 6 16 55" src="https://github.com/yaezzin/TIL/assets/97823928/39256271-baf5-414a-8472-52237c08e05f">

## 컨테이너 안에서의 작업

```
docker exec -it {컨테이너명} {명령어}
```

컨테이너와 지속적으로 연결하면서 작업을 하려면 ```docker exec -it {컨테이너명} /bin/bash``` 명령어를 사용해야 함
* 컨테이너 안에서 명령어를 내리는데 /bin/bassh 으로 실행시킬 수 있는 bash 쉘을 지속적으로 끊기지 않고 열어주는 명령어!
* 해당 명령어를 입력한 이후에는 # 대기 커서가 뜨면서 지속적으로 컨테이너에 명령이 내려짐

```
EX )

docker exec -it ws3 /bin/bash
cd htdoc
nano index.html # 내부 수정하고 다시 리로드하면 내용이 변경됨!
```

<img width="606" alt="스크린샷 2023-08-19 오후 6 40 13" src="https://github.com/yaezzin/TIL/assets/97823928/2b93b818-db8c-4c29-aa33-827d18d34883">

하지만 이전 방식은 실수로 컨테이너를 삭제한다면 모든 작업물이 날아가게 됨
* 따라서 호스트안에서 파일을 수정하면 컨테이너안에서 파일이 똑같이 수정되는 원격 작업 형태로 만들고, 이렇게 동기화되는 컨테이너를 여러개 만들어 둔다면 안전해짐!

```docker run -p 8888:80 -v ~/Desktop/htdocs:/usr/local/apache2/htdocs/ httpd```
* 8888번 포트를 컨테이너의 80포트로 연결시키는 포트포워딩
* Desktop 아래의 htdocs라는 디렉터리와 컨테이너 안에서 웹페이지를 찾기위해 약속된 디렉토리인 (~~/htdocs)를 서로 연결
  * ~/Desktop/htdocs 와 컨테이너의 /usr/local/htdocs/ 폴더를 동기화 시켜주는 아파치(httpd) 이미지를 사용하는 컨테이너를 생성 

<img width="427" alt="스크린샷 2023-08-19 오후 6 46 58" src="https://github.com/yaezzin/TIL/assets/97823928/810ff466-29f7-4772-94c1-a1d88ff78a61">

* 이후 호스트에서 그냥 만든 html 파일을 수정하고 리로드를 하면 아래와 같이 동기화가 됨

## 참고

* https://seosh817.tistory.com/345
* https://www.youtube.com/watch?v=SJFO2w5Q2HI&list=PLuHgQVnccGMDeMJsGq2O-55Ymtx0IdKWf&index=5
