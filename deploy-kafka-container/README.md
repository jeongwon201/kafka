# Docker 로 Kafka 컨테이너 배포

Docker 컨테이너로 Kafka 를 배포하는 과정을 설명합니다.   
<br />

Kafka 와 Zookeeper 는 공식으로 제공되는 컨테이너 이미지가 존재하지 않습니다.   
VMware 의 bitnami 에서 제공하는 이미지와 여러 환경 변수를 통해 간편하게 환경을 구성할 수 있습니다.
<br />

해당 예제는 **CentOS 7**을 기준으로 설명합니다.   
> CentOS 7 Docker 환경을 구성하려면 다음 링크를 참고하세요.   
> <a href="https://github.com/jeongwon201/docker/tree/main/docs/docker-1-env">CentOS 7, Docker 환경 구성</a>

<br />
<br />

Zookeeper 와 Kafka 컨테이너를 빠르게 구성하기 위해 **Docker Compose** 플러그인을 사용합니다.   
<br />
```yum install -y docker-compose-plugin``` 명령어로 Docker Compose 를 설치합니다.
<br />
<br />
<br />
<br />

## Apache Kafka 컨테이너 배포

```su -``` 명령어를 이용해 루트 계정으로 접속합니다.   
```mkdir kafka``` 명령어로 디렉토리를 생성 후, ```cd kafka``` 명령어로 kafka 디렉터리로 이동합니다.   
<br />

```vi docker-compose.yml``` vi 편집기를 이용해 docker-compose.yml 파일을 작성합니다.   
<br />

docker-compose.yml 내용은 다음과 같습니다.
```yaml
version: "3"
services:
  zookeeper:
    image: 'bitnami/zookeeper:3.6.3'
    ports:
      - '2181:2181'
    environment:
      - ALLOW_ANONYMOUS_LOGIN=yes
  kafka:
    image: 'bitnami/kafka:3.3.2'
    ports:
      - '9092:9092'
    environment:
      - KAFKA_BROKER_ID=1
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9092
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://127.0.0.1:9092
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - ALLOW_PLAINTEXT_LISTENER=yes
    depends_on:
      - zookeeper
```
<br />

**Zookeeper** 의 환경 변수에 대한 설명은 다음과 같습니다.
- **ALLOW_ANONYMOUS_LOGIN**
    - 인증되지 않은 사용자의 연결을 허용합니다.

**Kafka** 의 환경 변수에 대한 설명은 다음과 같습니다.
- **KAFKA_BROKER_ID**
    - Zookeeper 가 Kafka 브로커를 식별하기 위한 ID 값을 설정합니다.
- **KAFKA_CFG_LISTENERS**
    - 컨테이너 내부 통신을 위한 리스너를 설정합니다.
- **KAFKA_ADVERTISED_LISTENERS**
    - 컨테이너 외부에서 접속하기 위한 리스너를 설정합니다.
- **KAFKA_CFG_ZOOKEEPER_CONNECT**
    - Kafka 와 연동할 Zookeeper 를 설정합니다.
- **ALLOW_PLAINTEXT_LISTENER**
    - PLAINTEXT 리스너 사용을 허용합니다.

> 이 외 많은 환경 변수가 있으며, 필요하다면 Docker Hub 를 참고하세요.   
> <a href="https://hub.docker.com/r/bitnami/kafka">Docker Hub - bitnami/kafka</a>   
> <a href="https://hub.docker.com/r/bitnami/zookeeper">Docker Hub - bitnami/zookeeper</a>

<br />

```docker compose up -d``` 명령어로 컨테이너를 실행합니다.
<br />
<br />
<br />
<br />