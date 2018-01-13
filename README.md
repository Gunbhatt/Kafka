# Kafka
Load data to Kafka in to Topics and move to HDFS via Flume
**************************************************************
bin/zookeeper-server-start.sh config/zookeeper.properties

bin/kafka-server-start.sh config/server.properties 
bin/kafka-server-start.sh config/server1.properties 

vi config/server.properties --> change Broker ID & Listener and Log file path for server2 also  

bin/kafka-console-producer.sh --broker-list localhost:9092,localhost:9091 --topic Airports 
</home/acadgild/Downloads/airports.csv

bin/kafka-console-consumer.sh --bootstrap-server localhost:9091,localhost:9092 --topic Airports
--from-beginning

****************************FLUME ***************
 bin/flume-ng agent -n $agent_name -c conf -f conf/flume-conf.properties.templats

 bin/flume-ng agent --conf-file kafka.conf --name agent1 
*****************************
agent1.sources=kafka-source
agent1.channels=memory-channel
agent1.sinks=hdfs-sink 

agent1.sources.kafka-source.type = org.apache.flume.source.kafka.KafkaSource
 agent1.sources.kafka-source.zookeeperConnect = localhost:2181
 agent1.sources.kafka-source.topic = Airports
 agent1.sources.kafka-source.channels = memory-channel
 agent1.sources.kafka-source.interceptors = i1
 agent1.sources.kafka-source.interceptors.i1.type = timestamp
 agent1.sources.kafka-source.kafka.consumer.timeout.ms = 100
 
 agent1.channels.memory-channel.type = memory
 agent1.channels.memory-channel.capacity = 10000
 agent1.channels.memory-channel.transactionCapacity = 1000
 
 agent1.sinks.hdfs-sink.type = hdfs
 agent1.sinks.hdfs-sink.hdfs.path =/user/acadgild/Emirates/Raw
 agent1.sinks.hdfs-sink.hdfs.rollInterval = 5
 agent1.sinks.hdfs-sink.hdfs.rollSize = 0
 agent1.sinks.hdfs-sink.hdfs.rollCount = 0
 agent1.sinks.hdfs-sink.hdfs.fileType = DataStream
 agent1.sinks.hdfs-sink.channel = memory-channel
 
 
