# Deployment

> [공식 문서 이동](https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/#%EB%94%94%ED%94%8C%EB%A1%9C%EC%9D%B4%EB%A8%BC%ED%8A%B8%EC%9D%98-%EB%A1%A4%EC%95%84%EC%9B%83-%EA%B8%B0%EB%A1%9D-%ED%99%95%EC%9D%B8)

* 디플로이먼트(Deployment)는 파드와 레플리카셋(ReplicaSet)에 대한 선언적 업데이트를 제공한다.

## Deployment 생성

```yaml
# simple-deploy-svc.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  labels:
    app: web

spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: web
          image: teacherssamko/simple-web:v1
          ports:
            - containerPort: 8000
```
* ```spec.replicas``` 필드에 따라서 3개의 파드를 생성하기 위한 레플리카셋을 생성

<img width="885" alt="스크린샷 2023-09-12 오전 9 48 41" src="https://github.com/yaezzin/TIL/assets/97823928/5b78cad4-de21-46b7-b673-777a08f727d1">

* ```k apply -f simple-deploy-svc.yaml```를 수행하면 위와 같은 결과를 볼 수 있다.
* ```k get deployment```

<img width="885" alt="스크린샷 2023-09-12 오전 9 50 36" src="https://github.com/yaezzin/TIL/assets/97823928/afa87eb1-4b8a-4e8b-a869-34d70a679245">

* 각 파드에 자동으로 생성된 레이블을 보려면, ```kubectl get pods --show-labels```를 실행
* web이라는 레이블에 파드가 3개 생성된 것을 볼 수 있음

## Deployment 업데이트

```
k set image deploy web web=teacherssamko/simple-web:v2
```

<img width="885" alt="스크린샷 2023-09-12 오전 10 05 17" src="https://github.com/yaezzin/TIL/assets/97823928/00990a7d-e4be-4266-94cf-179e75211f04">

* ```k get rs```를 해보면 rs가 두개 생긴것을 볼 수 있음(하나=v1는 사라짐)
* 예전 버전의 파드를 점진적으로 새 버전으로 교체하여 서비스 중단을 최소화하면서 애플리케이션 업데이트를 수행하는 것을 의미 = ```Rolling Update```
* pod는 알아서 삭제해주지만 replicaset은 하나가 더생기고, 이전 것도 남아있음

## Deployment Rollback


```
k rollout undo deploy web
```
* 이제 현재 롤아웃의 실행 취소 및 이전 수정 버전으로 롤백 하기로 결정했다면 위의 명령어를 입력한다


```
kubectl rollout undo web --to-revision=2
```
* 특정 수정 버전으로 롤백하려면 --to-revision 옵션에 해당 수정 버전을 명시한다

## Deployment 일시 중지 및 재개

디플로이먼트를 업데이트할 때, 하나 이상의 업데이트를 트리거하기 전에 해당 디플로이먼트에 대한 롤아웃을 일시 중지할 수 있다. 변경 사항을 적용할 준비가 되면, 디플로이먼트 롤아웃을 재개한다

```
k rollout pause deploy web
k set image deploy web web=teacherssamko/simple-web:v4
k rollout resume deployment/nginx-deployment # 디플로이먼트 롤아웃을 재개
```

<img width="703" alt="스크린샷 2023-09-12 오전 10 15 24" src="https://github.com/yaezzin/TIL/assets/97823928/73dffbe8-d472-4ab1-ada1-41c6c026c1dc">



