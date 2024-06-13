# 컨슈머 그룹

#### 컨슈머 그룹 생성하기

```sh
// hello-group 이라는 consumer 그룹에 hello.kafka 토픽 설정
bin/kafka-consumer-groups.sh \
> --bootstrap-server localhost:9092 \
> --group hello-group \
> --topic hello.kafka \
> --reset-offsets --to-earliest --execute
```



#### 컨슈머 그룹 목록 확인하기

```bash
// localhost:9092 서버의consumer-group 목록에는 hello-group이 있음을 확인
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
hello-group
```



#### 컨슈머 그룹 상세정보 확인하기

```bash
// hello-group 컨슈머 그룹의 상세 정보 확인
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group hello-group --describe

Consumer group 'hello-group' has no active members.

GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
hello-group     hello.kafka     0          0               13              13              -               -   
```

&#x20;위의 실행 결과에서 주의 깊게 봐야  할 것은 LAG 값이다. LAG을 간단하게 설명하면 프로듀서에서 레코드를 저장할 때 생성되는 최신 오프셋(LOG\_END\_OFFSET)과 컨슈머가 현재 읽고 있는 오프셋(CURRENT\_OFFSET)의 차이이다. 즉 파티션에저장된컨레코드를컨슈머에서 지연없이 잘 처리하고 있는지 확인할 수 있는 지표로  이해하면 된다.&#x20;



&#x20; 그렇다면 consumer를 통해 레코드를 읽은 후 다시 컨슈머 그룹 정보를 확인하면 어떻게 될까?

```bash
// hello-group 컨슈머 그룹 내 hello.kafka 토픽의 레코드를 컨슈머를 통해 읽음
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic hello.kafka --group hello-group
hello
kafka
0
1
2
3
4
5
6
memessagevalue
new
hi
no3
```

```bash
// hello-group 컨슈머 그룹의 상세 정보 확인
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group hello-group --describe

GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                                 HOST            CLIENT-ID
hello-group     hello.kafka     0          13              13              0               consumer-hello-group-1-099e8f1c-5cfc-44e4-a211-9f149d4496a0 /127.0.0.1      consumer-hello-group-1
```

&#x20;hello-group 내 hello.kafka 토픽의 레코드를 컨슈머를 통해 읽은 후 다시 컨슈머 그룹의 상세 정보를 조회해보았다. 그랬더니 아까와 달리 LAG의 값이 0으로 변한 것을 확인할 수 있다.



#### 오프셋 리셋

```bash
// hello.kafka 토픽의 레코드를 consumer를 통해 다시 실행
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic hello.kafka --group hello-group
```

&#x20;이미 앞선 실습에서 consumer 를 통해 hello.kafka 토픽의 레코드를 가져오면서 offset 값을 최신으로 변경했기   때문에 위의 명령어를 실행해도 consumer는  아무 레코드를 가져올 수 없다. 이 때 오프셋 리셋 옵션을 이용하면 토픽의 레코드를 다시 읽을 수 있다.

```bash
// hello-group 컨슈머 그룹의 hello.kafka 토픽의 offset을 --to-earliest(가장 처음 0)으로 설정
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group hello-group --topic hello.kafka --reset-offsets --to-earliest --execute

GROUP                          TOPIC                          PARTITION  NEW-OFFSET
hello-group                    hello.kafka                    0          0
```

```bash
// hello.kafka 토픽의 레코드를 consumer를 통해 다시 실행, offset을 초기화했기 때문에 다시 읽어들임
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic hello.kafka --group hello-group
hello
kafka
0
1
2
3
4
5
6
memessagevalue
new
hi
no3
```
