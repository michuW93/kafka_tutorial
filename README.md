# kafka_tutorial

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



Topics should have a replication factor > 1 (usually 3).
Example:
![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/KafkaReplication.PNG?raw=true)

so every partition has leader and replications on another Brokers.
When there would be topic with 3 partitions and replication factor 2 then we would have every partition on 2 Brokers.
