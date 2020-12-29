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
10. you can opt for acks from producer( ack=1, ack=0 , ack=all)
11. produces can recover from error 
12. consumer-group is the group of consumer consuming a topic , they will have offset committed to a topic named "__consumer_offesets"




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

## kafka-console-producer

to start publishing to a topic with default properties 
```
./bin/kafka-console-producer.sh --broker-list 127.0.0.1:9092  --topic first_topic 

## this will return a prompt with ">" symbol , where you can type each message and press enter and once done, press CTRL+c to exit
```
this also has additional fields like --producer-property 
ex.
```
./bin/kafka-console-producer.sh --broker-list 127.0.0.1:9092  --topic first_topic --producer-property acks=all 
```
a kafka-console-producer creates a traffic once you send the first message -- shortcut to start sending data to a new topic 
in this case , only one replication factor and one partition is created

```
./bin/kafka-console-producer.sh --broker-list 127.0.0.1:9092  --topic second_topic
>whoami
[2020-12-29 16:22:22,681] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 3 : {second_topic=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
>whoareyou
>i am arjun 😁

```
always create topic with proper configs otherwise it will use 1 as default for partition and replication factor.
( you can change in server.properties for default) - num.partitions


## kafka-console-consumer 

doesn't read all the data before launching data . so consumer should be up and running to see the message produced by producer
```
./bin/kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic
```

to read all the message use "--from-beginning"

using group 
```
./bin/kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my_first_app_group
```

consumer groups dont work everytime with --from-beginning , since the offset is alreday committed to kafka for a group name . if you run the 
same command with same group name it will not read from the very beginning , 
however it does display all the messages that are produced afterwards. 


## kafka-consumers-groups

describe all-groups  ( similary , delete , describe single group using --group "group_name") 
```
./bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --all-groups
```

resettings offsets 
```
./bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group first_app_group --reset-offsets --to-earliest --execute --topic topic id #<has many optin>. --execute is necessary

--shift-by -2 . ( back 2 )
```














