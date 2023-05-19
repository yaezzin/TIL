## kafka에서 timeout 발생하는 경우

zookeeper를 실행하고, kafka를 실행한 이후 토픽을 생성하려고 했는데, 다음과 같은 에러가 발생했다.

```
ERROR org.apache.kafka.common.errors.TimeoutException: Timed out waiting for a node assignment. Call: listTopics (kafka.admin.TopicCommand$)
```

또한 카프카를 실행한 터미널 창을 보니, 다음과 같은 문제가 발생했다.

```
[2023-05-19 16:24:01,406] WARN [Controller id=0, targetBrokerId=0] Connection to node 0 (/218.38.137.27:9092) could not be established. Broker may not be available. (org.apache.kafka.clients.NetworkClient)
[20223-05-19 16:24:01,407] INFO [Controller id=0, targetBrokerId=0] Client requested connection close from node 0 (org.apache.kafka.clients.NetworkClient)
[2023-05-19 16:24:01,513] INFO [Controller id=0, targetBrokerId=0] Node 0 disconnected. (org.apache.kafka.clients.NetworkClient)
...
```

## 해결방법

```
$ cd 카프카가 있는 경로
$ cd config
$ vi server.properties

# 아래 주석 처리되어있는 것을 풀어주고, your.host.name에 localhost 또는 127.0.0.1 적어주기
# advertised.listeners=PLAINTEXT://your.host.name:9092 //  ip 주소 (ex: localhost)
```
#### 토픽이 생성된 것을 볼 수 있다.
<img width="753" alt="스크린샷 2023-05-19 오후 4 26 07" src="https://github.com/yaezzin/TIL/assets/97823928/b995e1e7-81f3-474a-8e3c-f143307a2692">
