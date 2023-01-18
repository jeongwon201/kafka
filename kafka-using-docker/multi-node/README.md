# 멀티 Kafka 브로커 컨테이너 배포

단일 Kafka 브로커 컨테이너 배포 과정입니다.   
<br />

```su -``` 명령어를 이용해 루트 계정으로 접속합니다.   
<br />

```yum install -y docker-compose-plugin``` 명령어로 Docker Compose 를 설치합니다.

```mkdir kafka``` 명령어로 디렉토리를 생성 후,   
```cd kafka``` 명령어로 kafka 디렉터리로 이동합니다.   
<br />

```vi docker-compose.yml``` vi 편집기를 이용해 docker-compose.yml 파일을 작성합니다.   
<br />

docker-compose.yml 내용은 다음과 같습니다.   
```yaml
version: '2'
services:
  zookeeper-1:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 22181:2181

  zookeeper-2:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 32181:2181

  zookeeper-3:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 42181:2181

  kafka-1:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper-1
      - zookeeper-2
      - zookeeper-3

    ports:
      - 29092:29092
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper-1:2181,zookeeper-2:2181,zookeeper-3:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka-1:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
  kafka-2:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper-1
      - zookeeper-2
      - zookeeper-3
    ports:
      - 39092:39092
    environment:
      KAFKA_BROKER_ID: 2
      KAFKA_ZOOKEEPER_CONNECT: zookeeper-1:2181,zookeeper-2:2181,zookeeper-3:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka-2:9092,PLAINTEXT_HOST://localhost:39092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
  kafka-3:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper-1
      - zookeeper-2
      - zookeeper-3
    ports:
      - 49092:49092
    environment:
      KAFKA_BROKER_ID: 3
      KAFKA_ZOOKEEPER_CONNECT: zookeeper-1:2181,zookeeper-2:2181,zookeeper-3:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka-2:9092,PLAINTEXT_HOST://localhost:49092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
```
<br />

**Zookeeper** 의 프로퍼티에 대한 설명은 다음과 같습니다.   
- **ZOOKEEPER_CLIENT_PORT**
  - 클라이언트가 Zookeeper 에 접속하기 위한 포트를 지정합니다.   
- **ZOOKEEPER_TICK_TIME**
  - Zookeeper Tick 단위 시간을 지정합니다.
<br />

**Kafka** 의 프로퍼티에 대한 설명은 다음과 같습니다.   
- **KAFKA_BROKER_ID**
  - Zookeeper 가 Kafka 브로커를 식별하기 위한 ID 값을 설정합니다.
- **KAFKA_ZOOKEEPER_CONNECT**
  - 연동할 Zookeeper 를 지정합니다.
- **KAFKA_ADVERTISED_LISTENERS**
  - 외부에서 접속하기 위한 리스너를 설정합니다.
- **KAFKA_LISTENER_SECURITY_PROTOCOL_MAP**
  - 보안을 위한 프로토콜 매핑을 설정합니다.
- **KAFKA_INTER_BROKER_LISTENER_NAME**
  - Kafka 브로커 간 통신에 사용될 리스너를 정의합니다.
- **KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR**
  - ISR(In Sync Replica), Replica 를 지정합니다.

<br />
<br />

```docker compose up -d``` 명령어로 컨테이너를 실행합니다.