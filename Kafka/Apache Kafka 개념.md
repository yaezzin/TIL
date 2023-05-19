# Kafka


<img width="700" alt="스크린샷 2023-05-19 오후 9 44 11" src="https://github.com/yaezzin/TIL/assets/97823928/d1aac72e-cd51-4fed-942f-cffd817efb89">

카프카는 Source Application과 Target Application간의 결합도를 줄이기 위해서 등장하였다. 
* Source Application 카프카에 데이터를 전달하면되고, Target Application은 카프카에서 데이터를 가져오면 된다.
* Source Application에서 전달할 수 있는 데이터 포맷은 json, tsv.. 등 다양하다.

# Broker 

<img width="556" alt="스크린샷 2023-05-19 오후 10 29 12" src="https://github.com/yaezzin/TIL/assets/97823928/37778664-3d8c-472f-a561-fa4de35e431c">

Kafka Broker는 카프카가 설치되어있는 서버 단위를 말한다. (하나의 서버라고 생각하자!)
* 보통 3개 이상의 브로커로 구성하여 사용하는 것을 권장한다.
* Broker가 여러개 모이면 Cluster를 이루게 된다.

# Topic

<img width="700" alt="스크린샷 2023-05-19 오후 9 44 44" src="https://github.com/yaezzin/TIL/assets/97823928/7a475d80-95d0-4deb-9aef-64ffc5809561">

Kafka는 각종 데이터를 담는 공간인 ```Topic```을 여러 개 가질 수 있다. 
* 카프카는 일종의 Queue라고 할 수 있으며, 그 안에 Topic을 넣는다고 보면 된다.
* Topic은 이름을 가질 수 있기 때문에, 무슨 데이터를 담는지 명확하게 담는다면 유지보수 시 편리하게 관리할 수 있다.

---

<img width="700" alt="스크린샷 2023-05-19 오후 9 57 47" src="https://github.com/yaezzin/TIL/assets/97823928/232dfe7b-4b3a-40bd-8e27-9dabb2ab5776">

* 하나의 Topic은 여러 개의 ```파티션```을 가질 수 있는데, 첫번째 파티션 번호는 0번부터 시작한다.
  * Topic은 파티션 맨 끝에서부터 차곡차곡 쌓이며, 오래된 순서대로 Consumer가 데이터를 가져가게 된다. 
  * Consumer가 레코드들을 가져가도 데이터는 삭제되지 않으며, 그대로 남아있는다
  * 남은 데이터는 새로운 Consumer가 다시 0번부터 가져갈 수 있다. 
  * 하지만 컨슈머 그룹이 달라야하고 ```auto.offset.rest = earliest```로 설정되어있어야 한다.
* 큐에 데이터를 넣는 역할은 Producer가, 큐에서 데이터를 가져가는 역할은 Consumer가 한다.

---

<img width="700" alt="스크린샷 2023-05-19 오후 10 04 09" src="https://github.com/yaezzin/TIL/assets/97823928/6db9db4f-8a6c-4a0b-bea3-a45e3975c555">

#### 파티션이 여러 개인 경우,
* Key를 지정하지 않고(= null), 기본 파티셔너를 사용할 경우 ```라운드 로빈```으로 할당
* Key가 있고, 기본 파티셔너를 사용할 경우 Key의 ```Hash```를 구하고, 특정 파티션에 저장한다.
* Partition은 처음 생성한 이후로는 추가할 수만 있고, 줄일 수는 없기 때문에 신중히 결정해야 한다.
  * Partition을 늘리게 되면 Consumer 개수를 늘려서 데이터 처리를 분산시킬 수 있다.
  * 또한 늘어난 Partition은 레코드가 저장되는 최대 시간과 크기를 지정하여 일정한 기간, 용량 동안 데이터를 저장하고, 삭제될 수 있다. 

# Record

각 파티션은 일련의 순차적인 메시지들로 구성되는 레코드를 포함한다.
* Record는 ```키(Key)```, ```값(Value)```, ```타임스탬프(Timestamp)```로 구성되어있다.
  * Key : 레코드를 식별하기 위한 값으로, 키의 해시값을 토대로 파티션 할당에 사용된다.
* 각 레코드는 파티션 내에서 고유한 오프셋(Offset)을 가지며, 컨슈머는 이 오프셋을 사용하여 특정 레코드를 읽거나 처리할 수 있다.

# Replication, ISR

<img width="679" alt="스크린샷 2023-05-19 오후 10 12 25" src="https://github.com/yaezzin/TIL/assets/97823928/5e155dc5-f26b-4761-bc3a-25cf9ec286a9">

### Replication

브로커에게 Replication 팩터 수 만큼 토픽안의 ```파티션들을 복제```하도록 하는 설정 값
* Replication은 브로커 개수보다 많을 수 없다. (즉 브로커가 3개라면 Replication의 값도 3까지만 가능)
* Replication은 브로커가 사용 불가 상태가 된다면, Replication을 통해 Flower파티션이 존재하기 때문에 남은 Follow 파이션이 Leader 파티션 역할을 승계하게 된다.
* Replication의 개수가 많이지게 되면 그만큼 브로커의 리소스 사용량도 늘어나게 된다. 따라서 카프카에 들어오는 데이터량과 저장시간을 고려하여 Replication 개수를 설정해야한다. (3대 이상의 브로커를 사용할 때 Replication 개수는 3개가 좋다.)

### ISR

원본 1개의 파티션은 Leader 파티션이라고 부르고, 나머지 파티션을 Follower 파티션이라고 한다.
* 이 Leader, Follower 파티션을 합쳐서 ```ISR(In Sync Replica)```라고 볼 수 있다.

### Ack Option

<img width="679" alt="스크린샷 2023-05-19 오후 10 34 33" src="https://github.com/yaezzin/TIL/assets/97823928/3cceb8c1-37a4-4133-ab87-538aff3795e1">

프로듀서에는 ```Ack```라는 상세 옵션이 있다. 0, 1, all 3개의 옵션을 사용할 수 있다
* 0인 경우 프로듀서는 리더 파티션에 데이터를 전달하고, 응답값을 전달받지 않는다. 따라서 리더 파티션에 데이터가 정상적으로 전송되었는지 나머지 파티션에 정상적으로 복제되었는지 알 수 없다. 따라서 속도는 빠르나 데이터 유실 가능성이 있다.
* 1일 경우 리더 파티션에 정상적으로 데이터가 전달되었는지 응답값을 받을 수 있다. 하지만 나머지 파티션에 복제되었는지는 알 ㅜ 없다. 만약 리더 파티션이 데이터를 받은 즉시 브로커에 장애가 난다면 나머지 파티션에 데이터가 미처 전송되지 못한 상태이므로 여전히 데이터 유실가능성이 있다.
* all일 경우 1옵션에 팔로워 파티션에 잘 전달되었는지까지 응답값을 받는다. 이 방식은 데이터 유실은 없으나, 속도가 느리다는 단점을 가진다.


## 참고
* [데브원영 아파치 카프카](https://www.youtube.com/watch?v=waw0XXNX-uQ&list=PL3Re5Ri5rZmkY46j6WcJXQYRlDRZSUQ1j)
