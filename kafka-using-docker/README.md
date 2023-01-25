# Docker 로 Kafka 컨테이너 배포

Docker 컨테이너로 Kafka 를 배포하는 과정을 설명합니다.   
<br />

Zookeeper 와 Kafka 컨테이너를 빠르게 구성하기 위해 **Docker Compose** 플러그인을 사용합니다.   
<br />

Docker 컨테이너로 Kafka 를 배포하는 과정을 2 가지로 나누어 설명합니다.   
- **단일 Kafka 브로커 구성**
- **멀티 Kafka 브로커 구성**
<br />

**단일 Kafka 브로커**는 주로 로컬 개발 환경에서 사용됩니다.   
Zookeeper 컨테이너와 Kafka 컨테이너로 구성합니다.   
<br />

**멀티 Kafka 브로커**는 주로 운영 환경에서 사용됩니다.   
여러 대의 Kafka 브로커로 구성한 것을 **Kafka 클러스터** 라고 합니다.   
최소 3 대 이상으로 구성하는 것을 권장하며, 각 3 대의 Zookeeper 컨테이너와 Kafka 브로커 컨테이너로 구성합니다.   
<br />

해당 예제는 **CentOS 7**을 기준으로 설명합니다.   
> CentOS 7 Docker 환경을 구성하려면 다음 링크를 참고하세요.   
> <a href="">CentOS 7, Docker 환경 구성</a>
<br />

CentOS 7, Docker 환경이 구성되었다면 다음 링크로 이동하세요.   
<a href="https://github.com/jeongwon201/kafka/tree/main/kafka-using-docker/single-node">단일 Kafka 브로커 구성하기</a>   
<a href="https://github.com/jeongwon201/kafka/tree/main/kafka-using-docker/multi-node">멀티 Kafka 브로커 구성하기</a>
