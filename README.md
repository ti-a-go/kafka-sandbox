# Rodando o Kafka dentro de um conteiner Docker

[Imagem oficial no docker hub](https://hub.docker.com/r/apache/kafka)

[Documentação Kafka](https://kafka.apache.org/documentation/)

## How does Kafka work in a nutshell?

Kafka is a **distributed system** consisting of **servers and clients** that communicate via a high-performance **TCP network protocol**.

#### Servers

Kafka is run as a **cluster of one or more servers** that can span multiple datacenters or cloud regions.

Some of these servers form the **storage layer**, called the **brokers**. 

Other servers run Kafka Connect to continuously import and export data as event streams to integrate Kafka with your existing systems such as relational databases as well as other Kafka clusters. 

A Kafka cluster is **highly scalable and fault-tolerant**: if any of its servers fails, the other servers will take over their work to ensure continuous operations without any data loss.

#### Clients

They allow you to write distributed applications and microservices that **read, write, and process streams of events in parallel, at scale, and in a fault-tolerant manner** even in the case of network problems or machine failures. 

Kafka ships with some such clients included, which are augmented by dozens of clients provided by the Kafka community: clients are available for Java and Scala including the higher-level Kafka Streams library, for Go, Python, C/C++, and many other programming languages as well as REST APIs. 

# Criando o container manualmente

Iniciar o container

```bash
docker run -d --name broker -p 9092:9092 apache/kafka:latest
```

Abrir o container interativamente

```bash
docker exec --workdir /opt/kafka/bin/ -it broker sh
```

Criar um tópico

```bash
./kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test-topic
```

Enviar uma mensagem para o tópico criado acima

```bash
./kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test-topic
```

Ler as mensagens criadas acima

```bash
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning
```


# Comandos Importantes

Iniciar o servidor do Zookeeper

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

Iniciar o servidor do Kafka

```bash
bin/kafka-server-start.sh config/server.properties
```

Criar tópico

```bash
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic LOJA_NOVO_PEDIDO
```

Listar tópicos do servidor

```bash
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

Enviar mensagens no tópico criado acima

```bash
bin/kafka-console-producer.sh --boker-list localhost:9092
```

Consumir as mensagens publicadas no tópico

```bash
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic LOJA_NOVO_PEDIDO --from-beginning
```


# Python Kafka Client

[Biblioteca](https://kafka-python.readthedocs.io/en/master/apidoc/KafkaConsumer.html)

[Referência](https://www.instaclustr.com/support/documentation/kafka/using-kafka/use-kafka-with-python/)


Producer

```python
from kafka import KafkaProducer


producer = KafkaProducer(
    bootstrap_servers="0.0.0.0:9092",
)

producer.send("my-topic", "my-message")
producer.flush()
```

Consumer

```python
from kafka import KafkaConsumer


consumer = KafkaConsumer(
    "my-topic",
    bootstrap_servers="0.0.0.0:9092",
    group_id="my-group",
)
```


# REFERÊNCIAS

[Wikipedia](https://en.wikipedia.org/wiki/Apache_Kafka)

[Documentação Kafka](https://kafka.apache.org/documentation/)

[Zookeeper](https://zookeeper.apache.org/)

