# kafka_tutorial
following tutorial of Stephane Maarek

![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/topic_anatomy.png?raw=true)

- Topic in kafka has 3 partitions.
- Offset is like id = partition 1 offset 2, means 2nd element in partition 1.
- Order is guaranteed only within a partition (not across partitions).
- Data is kept for some time, default is one week.
- Data is immutable, once written can't be changed.
- Data is assigned randomly to a partition unless a key is provided.

Example:
We have two topics:
- Topic-1 with 3 partitions
- Topic-2 with 2 partitions

![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/topics_to_broker.png?raw=true)

if we would have Topic-3 with 4 partittions then two partitions would be in the same broker.


## REPLICATION
Topics should have a replication factor > 1 (usually 3). So if the leader dies then other brokers still can serve data.
Example:
![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/KafkaReplication.PNG?raw=true)

so every partition has leader and replications on another Brokers.
When there would be topic with 3 partitions and replication factor 2 then we would have every partition on 2 Brokers.

At any time only one broker can be a leader for a partition and only that leader can receive and serve data for a partition. Other brokers will synchronize the date.

Each partition has one leader and many ISR

## Producers

Producers are writing into topics.
Producers can choose to receive acknowledgemet of data writes:
- acks=0: producer wont wait for acknowledgement (possible data loss)
- acks=1 (default): Producer will wait for leader acknowledgement (limited data loss)
- acks = all: leader and all replicas acknowledgement (no data loss)
 

Message keys:
Producers can choose to send a key with message (key can be anything string, number etc.)
- If key = null then data is sent round robin
- If key is sent then all messages for that key will always go to the same partition

 
## Consumer

![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/kafka_consumers.png?raw=true)

If there are more consumer than partitions then some consumers will be inactive

## Offsets

Kafka stores the offsets at which a consumer group has been reading.
Offsets comitted live in a Kafka topic named __consumer__offsets.

When consumer from consumer group has processed data received from Kafka then should commit offsets so then f consumer dies it's know how much were processed because of commited __consumer__offsets.

Consumer choose when to commit offsets and there are 3 ways:
- At most once - offset is commited as soon as message is received but when something goes wrong message will be lost, wont be processed again
- At least once - offsets are committed after the message is processed, if processing goes wrong then message will be read again but we need to remember then it can result in duplicates so we have to provide idempotent processing.
- Exactly once - only Kafka for Kafka


Every kafka broker is called bootstrap server.
If we connect to one broker we are connected to the entire cluster because each broker knows everything about other brokers.

Connection:

Kafka client -------------connection+metadata request ----> kafka cluster where are all brokers

Kafka client <------------- list of all brokers -------------------kafka cluster

Kafka client now can connect to needed brokers



Zookeeper - manages brokers, helps in performing leader election, send notification to kafka when new topic arrives, broker dies etc. Kafka cant work without zookeeper. Zookeeper always operates on odd number of servers 3,5,7…

 

 

Messages are appended to a topic-partition in the order they are sent.

Consumers read messages in the order stored in a topic-partition.

With a replication factor of N, producers and consmers can tolerate up to N-1 brokers being down. Replication factor 3 is good idea (allows for two broker to die).

As long as the number of partitions remains constant for a topic (no new partition) the same key will always go to the same partition.

If factor replication is 3 then every topic will live on 3 brokers.

## Run kafka
Download and add into PATH - open .bashrc file and add at the end: `export PATH=/home/michu/kafka_2.13-2.8.0/bin:$PATH` then in terminal `source .bashrc`

1. start zookeeper: zookeeper-server-start.sh config/zookeeper.properties
2. start kafka: kafka-server-start.sh config/server.properties

## Run producer
To run producer:
`kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic first_topic` or more options `kafka-console-producer.--broker-list 127.0.0.1:9092 --topic first_topic --producer-property acks=all`.

To check current topics `kafka-topics.sh --zookeeper 127.0.0.181 --list` and to check one of topics `kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic topic_name --describe`

## Run consumer and consumers in group
`kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic` but this won't read old messages, only the new one. To see all messages:
`kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --from-beginning`

Example:
![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/kafka_messages.png?raw=true)

as we can see without `--from-begining` consumer won't display previous messages, only those who are added when consumer is up will be displayed.

## Consumers in group (group id - name of app)
![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/many_consumers.png?raw=true)
as we can see here, that messages are split between consumers because we have 3 partitions and two consumers. When we would have 3 consumers then messages would be split between 3 consumers. If one would die then again messages would go to two consumers. That's consumers group.


To check consumer groups: `kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list`
![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/consumer_group.png?raw=true)

on the screen above LAG for `first_topic` is 2 which means that there are two unread messages.
