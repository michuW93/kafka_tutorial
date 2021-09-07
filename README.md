# kafka_tutorial

![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/topic_anatomy.png?raw=true)

- Topic in kafka has 3 partitions.
- Offset is like id = partition 1 offset 2, means 2nd element in partition 1.
- Order is guaranteed only within a partition (not across partitions).
- Data is kept for some time, default is one week.
- Data is immutable, once written can't be changed.
- Data is assigned randomly to a partition unless a key is provided.

Example:
We have three topics:
- Topic-A with 3 partitions
- Topic-B with 3 partitions
- Topic-C with 4 partitions

![alt text](https://github.com/michuW93/kafka_tutorial/blob/master/topic_to_broker.png?raw=true)


Broker 1                                   Broker 2                                Broker 3

Topic-A partition0                    Topic-A partition2                     Topic-A partition1

Topic-B partition1                    Topic-B partition0

Topic-C partition3

Topic-C partition4                    Topic-C partition 0                    Topic-C partition1


for Topic-C with 4 partitions there are two partitions in Broker 1.
