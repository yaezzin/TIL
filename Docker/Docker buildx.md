## Docker Buildx

* MAC M1 OS는 기본적으로 arm기반 아키텍처이기 때문에 도커파일을 빌드하여 도커이미지를 생성하면 linux/arm64으로 생성됨
* 하지만 대부분 linux/amd64 이기 때문에 M1에서 빌드한 이미지를 사용하려면 아래와 빌드 단계에 서--platform 옵션으로 linux/arm64로 지정해줘야함!

## Buildx

Buildx를 사용하면 여러 아키텍처와 플랫폼에 대한 이미지를 동시에 빌드할 수 있으며, 크로스 플랫폼 개발 및 배포에 유용!

### docker buildx 확인

<img width="671" alt="스크린샷 2023-09-12 오전 11 15 45" src="https://github.com/yaezzin/TIL/assets/97823928/aab399a7-09f7-4069-bf6a-0eecf39b52ec">

### create builder

```
# docker buildx create --name [builder instance 명] --driver [드라이버 이름] --use
$ docekr buildx create m1builder --dirver linux/arm64, linux/amd64 -- use

$ docker buildx build --platform linux/amd64 --tag ${image} .
```
