## 테라폼 Error acquiring the state lock 해결법

### 문제 상황

테라폼에서 ```terraform apply```를 하는 경우 Lock 에러가 발생

<img width="694" alt="스크린샷 2023-08-23 오전 11 21 24" src="https://github.com/yaezzin/TIL/assets/97823928/070904d4-619a-44b0-87b9-8cf45095b723">


```
Terraform acquires a state lock to protect the state
from being written by multiple users at the same time.
```
* 에러를 살펴보면 테라폼이 여러 사용자가 동시에 사용하는 것을 방지하기 위해 락을 걸었다고 나와있음


### 해결법 

```
ps aux | grep terraform
```

<img width="1003" alt="스크린샷 2023-08-23 오전 11 25 41" src="https://github.com/yaezzin/TIL/assets/97823928/dad252cf-6d6b-45b1-9a6a-859c2ef5d1a4">

* 위의 명령어로 프로세스 상태를 살펴보면, apply와 plan이 계속해서 돌아가는 것을 볼 수 있음
* 따라서 프로세스를 Kill 해주면 된다!
