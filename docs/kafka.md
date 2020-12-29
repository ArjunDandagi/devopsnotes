# KAFKA ( linkedin learning course , by stephane mareek)

1. install from apache. https://kafka.apache.org/downloads
2. add them to path  (all the binaries are stored in bin this software is also packed with .bat files for windows , we can remove them if not needed )
3. add the bin to $PATH

NOTES - theory
1. TOPIC is the main concept in kafka
2. Broker are the nodes
3. consumers are the application or process that consume the message from a topic
4. Partition is a logical separation of a kafka topic
5. replcation factor - no of redundant copies of data ( atleast 1 , exactly 1 , at most 1 ) 
6. zookeeper. ( metadata holder ) - kafka can't work without zookeeper : default port 2181 
7. replication-factor can never be greater than the no of Broker nodes
8. each partition will always have a leader ( and then inactive sync replica)
9. zookeeper manages the leader election in case nodes goes down , comes up etc



## Topics 

create a topic 
```
./bin/kafka-topics.sh --zookeeper 127.0.0.1:2181  --topic first_topic --create --partitions 3 --replication-factor 1
```

describe a topic 
```
./bin/kafka-topics.sh --zookeeper 127.0.0.1:2181  --topic first_topic  --describe 

output:
 Topic: first_topic	PartitionCount: 3	ReplicationFactor: 1	Configs: 
	Topic: first_topic	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
	Topic: first_topic	Partition: 1	Leader: 0	Replicas: 0	Isr: 0
	Topic: first_topic	Partition: 2	Leader: 0	Replicas: 0	Isr: 0
```

list topics 
```
./bin/kafka-topics.sh --zookeeper 127.0.0.1:2181  --list 
```

delete topic
```
./bin/kafka-topics.sh --zookeeper 127.0.0.1:2181  --topic first_topic --delete

output:
Topic first_topic is marked for deletion.
Note: This will have no impact if delete.topic.enable is not set to true.

```




