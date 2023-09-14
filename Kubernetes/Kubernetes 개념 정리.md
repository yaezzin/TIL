# Kubernetes

## 컨테이너와 가상머신 차이

<img width="802" alt="스크린샷 2023-09-14 오전 10 16 53" src="https://github.com/yaezzin/TIL/assets/97823928/a7f62a07-2c0f-4342-80eb-00aeae754fba">

### 가상머신

* VM의 경우 호스트OS에 의해 VM을 가상화 시켜주는 하이퍼바이저(virtual box, VMware)들이 있음
* ```하이퍼 바이저```를 사용하여 원하는 운영체제로 Guest OS를 올려 여러 개의 VM을 만들 수 있음
* GuestOS도 HostOS와 같이 하나의 OS를 독립적으로 가지고 있는 것처럼 사용 가능
* 각 VM은 자체 운영 체제와 커널을 가지고 있으며, 무겁고 느리다는 단점을 지니나 여러 운영 체제와 어플리케이션을 호스팅하기에 유연함

### 컨테이너

> 앱이 구동되는 환경까지 감싸서 실행할 수 있도록 하는 격리 기술

* 호스트 OS의 리소스를 논리적으로 분리시켜 공유하고, 격리된 환경을 제공하여 여러 프로그램을 구동하더라도 서로 간섭하는 문제가 생기지 않도록 하는 도구
* 프로그램을 구동하는데 필요한 환경을 격리시켜서 여러 프로그램을 구동하더라도 서로 간섭하는 문제가 생기지 않도록 하는 도구
* 컨테이너는 가상머신과 달리 자체 운영체제와 커널을 가지고 있지 않기 때문에 가볍고 빠름

### 컨테이너 런타임

> 컨테이너를 다루는 도구 ex ) 도커

* 컨테이너 런타임은 컨테이너 이미지를 기반으로 컨테이너를 생성하고 실행하는 소프트웨어
* 컨테이너 이미지에 포함된 파일 시스템 및 실행 환경을 실제로 실행 가능한 프로세스로 구동하는 역할을 함

### 컨테이너 오케스트레이션

* 도커 컨테이너로 서비스를 하게 되면 하나의 도커 이미지 안에 서비스 운영에 필요한 모든 것들이 들어 있어 개발자들이 손쉽게 협업을 할 수 있고,
  배포가 쉽고 빠르며 시스템 의존성을 쉽게 업그레이드할 수 있어 스케일 아웃에 용이함
* 하지만 이렇게 컨테이너화된 애플리케이션도 역시 관리를 해야하며, 컨테이너화된 어플리케이션이 다운이 되면 직접 재실행 시켜야 함!
  -> 전통적인 방식과 VM보다 관리가 용이하지만 컨테이너의 스케일 아웃 장점 때문에 관리해야 하는 컨테이너 수가 많아지게 되면 ...? => 쿠버네티스 사용하자!

# Kubernetes

## 구성요소

<img width="702" alt="스크린샷 2023-09-14 오후 1 26 44" src="https://github.com/yaezzin/TIL/assets/97823928/e97b8436-f229-4b76-be0b-6ae2280a2596">

## 1. 컨트롤 플레인 (Control Plane)

<img width="367" alt="스크린샷 2023-09-14 오후 1 30 13" src="https://github.com/yaezzin/TIL/assets/97823928/0c080653-d9ba-402a-9200-4c99e572b57c">

### 1-1. kube-apiserver 

* kube-apiserver는 쿠버네티스 클러스터로 들어오는 요청을 받아들이고, 사용자와 컨트롤 플레인 내의 요소들과 통신함 
* 예를 들어 kubectl을 사용해 각종 명령을 수행할 경우 이 명령은 kube-apiserver로 전송됨
* 이렇게 전달된 요청에 대하여 kube-apiserver는 이 요청의 처리 흐름에 따라 적절한 컴포넌트로 요청을 전달하는 역할까지 함

### 1-2. etcd 

* 모든 클러스터 데이터를 저장하는 분산 데이터 저장소(storage)
* 쿠버네티스 클러스터가 동작하기 위해서는 클러스터 및 리소스의 구성 정보, 상태 정보 및 명세 정보 등이 필요하기 때문에 etcd는 이를 키-값(key-value) 형태 저장
  
### 1-3. kube-scheduler 

* 새로 생성된 파드를 감지하여 어떤 노드로 배치할지 결정하는 작업을 kube-scheduler가 담당함

### 1-4. kube-controller-manager

* 클러스터의 다양한 컴포넌트들의 상태를 감지하고, 원하는 상태로 만드는 역할
* 다운된 노드가 없는지, 파드가 의도한 복제(Replicas) 숫자를 유지하고 있는지, 서비스와 파드는 적절하게 연결되어 있는지, 
  네임스페이스에 대한 기본 계정과 토큰이 생성되어 있는지를 확인하고 적절하지 않다면 적절한 수준을 유지하기 위해 조치

## 2. Node

<img width="391" alt="스크린샷 2023-09-14 오후 2 06 03" src="https://github.com/yaezzin/TIL/assets/97823928/d470d97a-695a-495f-bdee-25f4b2e054f2">

### 2-1. kubelet 

* 각각의 노드에서 실행되며, 컨트롤 플레인의 kube-apiserver와 통신하고 파드에서 컨테이너가 동작하도록 관리합니다.
* YAML을 쿠버네티스 클러스터에 적용하기 위해 kubectl 명령어를 사용할 때, 이 YAML이 kube-apiserver로 전송된 후 kubelet으로 전달됨
* kubelet은 이 YAML을 통해 전달된 파드를 생성 혹은 변경하고, 이후 이 YAML에 명시된 컨테이너가 정상적으로 실행되고 있는지 확인

### 2-2. container runtime 

* 파드에 포함된 컨테이너 실행을 실질적으로 담당하는 애플리케이션을 의미 - 보통 Docker 사용

### 2-3. kube-proxy 

* 쿠버네티스 클러스터 내부에서 네트워크 요청을 전달하는 역할

# Kubernetes Object 

<img width="708" alt="스크린샷 2023-09-14 오후 2 21 55" src="https://github.com/yaezzin/TIL/assets/97823928/04674ced-9bec-4747-8392-a617af902e83">

## 1. Pod

<img width="420" alt="스크린샷 2023-09-14 오후 2 19 15" src="https://github.com/yaezzin/TIL/assets/97823928/fc09bc13-f6ce-47b8-8b63-a93783622dc5">

* 다수의 컨테이너를 pod이란 곳에 묶어서 관리!
* 각 pod은 자체 IP, host name, process이 있는 논리적으로 분리된 머신으로, Pod은 프로세스를 실행하는 하나 이상의 컨테이너를 가짐

### Pod 주요 명령어

* ```kubectl get pods``` : 클러스터 내 파드 목록 조회 (default 네임스페이스)
* ```kubectl get pods -n <namespace>``` : 클러스터 내 특정 네임스페이스의 파드 목록 조회
* ```kubectl get pods -o wide``` : 파드 목록의 상세 조회(사설IP, 노드정보 포함)
* ```kubectl edit pod <pod-name>``` : 파드를 수정
* ```kubectl describe pod <pod-name>``` : 파드의 상세 정보 확인
* ```kubectl logs <pod-name>``` : 파드 내 구동 중인 컨테이너의 로그 확인
* ```kubectl exec -it <pod-name> -- /bin/sh``` : 파드의 컨테이너에 접속하여 sh를 대화형으로 실행
* ```kubectl delete pod <pod-name>``` : 파드 삭제


## 2. NameSpace

<img width="524" alt="스크린샷 2023-09-14 오후 2 44 22" src="https://github.com/yaezzin/TIL/assets/97823928/ee9701c9-8ba2-4da0-8c91-c6f01c1ffd23">

> 클러스터를 논리적으로 분리한 것

* Pod, Service 등은 네임 스페이스 별로 생성이나 관리가 될 수 있고, 사용자의 권한 역시 이 네임 스페이스 별로 나눠서 부여할 수 있음
* 또한 네임스페이스별로 리소스의 할당량을 지정할 수 있고, 리소스를 나눠서 관리할 수 있음
* 즉 하나의 클러스터 내에 dev/prod/test 환경을 분리하고자 할 때, 3개의 네임 스페이스로 나눠 운영할 수 있음

## 3. Service

<img width="682" alt="스크린샷 2023-09-14 오후 2 45 41" src="https://github.com/yaezzin/TIL/assets/97823928/46c615fd-84f7-4e3d-a486-b6291a737f83">

* pod끼리는 내부에서 서로 접속이 가능하지만 외부에선 접속이 불가능하므로, 내부/외부에서 둘다 접속하고자 할 때는 service를 사용
* Pod에 Label을 붙이고, Label Selector로 선택하여 하나의 endpoint로 노출
* ```IP, Port 관리``` : pod가 죽었을 때, pod은 Replication Controller에 의해 새로운 pod으로 대체되고, 새로운 IP와 port를 부여받음
* ```단일 endpoint``` : Service가 생성되면 static ip를 할당받고 service가 존재하는 동안 변경되지 않음. 따라서 pod에 직접 연결하는 대신 클라이언트는 service의 IP주소와 port를 통해 연결

```
apiVersion: v1
kind: Service
metadata:
  name: example-lb
spec:
  type: LoadBalancer # (LoadBalancer, Nodeport, ClusterIp ...)
  selector:
    app: example
  ports:
    - port: 80
      targetPort: 8000
```

```NodePort```
* 외부에서 노드의 IP 주소와 NodePort를 사용하여 서비스에 접근 가능
* 클라이언트의 요청은 노드로 직접 전달

```LoadBalancer```
* 외부에서 단일 IP 주소 또는 도메인을 사용하여 서비스에 접근
* 로드 밸런서는 클러스터 외부에서 들어오는 트래픽을 여러 노드 또는 Pod 사이에 분산
   
## 4. Storages

### PV (Persistent Volume)
 
* 만약 컨테이너가 꺼지고 삭제가 되면, 컨테이너 내부에 있던 파일들이 삭제되기 때문에  영구적인 파일 저장을 위해선 PV를 사용해야함  

### PVC 생성

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: db-pod-pvc

spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: nks-block-storage
```
* accessModes : 사용하고자 하는 PV의 accessModes와 동일한 옵션을 사용해야 bound 가능
* requests : 사용을 원하는 볼륨의 요구조건을 명시
* storage : 사용하고자 하는 최소한의 크기로서 명시한 용량보다 큰 PV도 상관 없음

### Pod에 마운트

```yaml
... (생략)
spec:
  ... 
  volumes:
    - name: db-data
      persistentVolumeClaim:
        claimName: db-pod-pvc
```

## 5. Controller

### ReplicaSet

* Pod을 여러 개(한 개 이상) 복제하여 관리하는 오브젝트로 Pod을 생성하고 개수를 유지하려면 반드시 ReplicaSet을 사용해야 함
* 파드가 띄워져있는 워커노드 하나가 죽으면 RepliacaSet은 의도한 Pod 개수를 충족하기 위해 다른 워커 노드에 새로운 Pod를 띄워 개수를 맞춰줌
* ReplicaSet은 복제할 개수, 개수를 체크할 label selector, 생성할 Pod의 Spec 등을 가지고 있음

### Deployment

* Deployment는 ReplicaSet을 관리하며 다른 유용한 기능과 함께 Pod에 대한 선언적 업데이트를 제공하는 상위 개념!
* 결론적으로 직접 ReplicaSet & Pod 종류의 오브젝트 yaml 파일을 작성 하는 대신 하위로 ReplicaSet & Pod 내용을 모두 포함하는 Deployment 오브젝트 yaml 파일만 작성하면 됨
 
## 참고

* https://www.samsungsds.com/kr/insights/kubernetes-3.html
* https://chacha95.github.io/2020-08-23-Docker_Kubernetes4/
