# Topic

### Topic 생성

```
$KAFKA_HOME/bin/kafka-topics.sh --create --topic {토픽 이름} --bootstrap-server localhost:9092\ --partitions 1
```

### Topic 삭제

```
$KAFKA_HOME/bin/kafka-topics.sh --delete --topic {토픽 이름} --bootstrap-server localhost:9092
```


### Topic 리스트 확인

```
$KAFKA_HOME/bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```

### Topic 상세정보


```
$KAFKA_HOME/bin/kafka-topics.sh --describe --topic {토픽 이름} --bootstrap-server localhost:9092
```

### Topic 파티션 수 변경

```
$KAFKA_HOME/bin/kafka-topics.sh --alter --topic {토픽 이름} --bootstrap-server localhost:9092 --partitions {새로운 파티션 수}
```

### Message 생산

```
$KAFKA_HOME/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic {토픽 이름}
```

### Message 소비

```
$KAFKA_HOME/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic {토픽 이름} \ --from-beginning
```

<img width="1512" alt="스크린샷 2023-05-19 오후 4 33 04" src="https://github.com/yaezzin/TIL/assets/97823928/f6bf4de1-a827-4c00-8e3e-ea154b7b9232">
