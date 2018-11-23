# Part C - Quantitative Analyzing
实验利用Kafka自带的性能测试脚本kafka-producer-perf-test.sh和kafka-consumer-perf-test.sh测试Kafka的性能。
## 生产者测试
###  Throughput与Message size的关系
- 实验条件
   1个Broker，1个Topic，6个Partition，1个Replication，3个Producer
- 测试项目
   分别测试消息长度为100，200，400，800，1000，2000，5000字节时的集群总吞吐量
- 创建Topic
    > ./bin/kafka-topics.sh –create –zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181      –replication-factor 1 –partitions 6 –topic D1
- 测试脚本
    > ./bin/kafka-producer-perf-test.sh --topic D1 --num-records 10000  --throughput 10000 --reco-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    > ./bin/kafka-producer-perf-test.sh --topic D1 --num-records 10000  --throughput 10000 --reco-size 200 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    > ./bin/kafka-producer-perf-test.sh --topic D1 --num-records 10000  --throughput 10000 --reco-size 400 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    > ./bin/kafka-producer-perf-test.sh --topic D1 --num-records 10000  --throughput 10000 --reco-size 800 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    > ./bin/kafka-producer-perf-test.sh --topic D1 --num-records 10000  --throughput 10000 --reco-size 1000 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    > ./bin/kafka-producer-perf-test.sh --topic D1 --num-records 10000  --throughput 10000 --reco-size 2000 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667　
    > ./bin/kafka-producer-perf-test.sh --topic D1 --num-records 10000  --throughput 10000 --reco-size 5000 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
- 测试结果
Message size | MB/S | Records/S
	----|------|----
	100 | 60.86  | 638162.0932
	200 | 88.25  | 486381.3231
	400 | 121.41  | 318268.6178
	800 | 150.18  | 196695.5153
    1000 | 146  | 153092.4679
    2000 | 152.43  | 77011.4
    5000 | 159.73  | 33497.4709
- 测试结论
由结果可知，消息越长，每秒所能发送的消息数越少，而每秒所能发送的消息的量（MB）越大。另外，每条消息除了Payload外，还包含其它Metadata，所以每秒所发送的消息量比每秒发送的消息数乘以100字节大，而Payload越大，这些Metadata占比越小，同时发送时的批量发送的消息体积越大，越容易得到更高的每秒消息量（MB/s）。

### Throughput与Partition number的关系
- 实验条件
   1个Broker，1个Topic，1个Replication，3个Producer，消息Payload为100字节
- 测试项目
   分别测试1到9个Partition时的吞吐量 
- 创建Topic
    > ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 1 --topic p1
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 2 --topic p2
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 3 --topic p3
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 4 --topic p4
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 5 --topic p5
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 6 --topic p6
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 7 --topic p7
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 8 --topic p8
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 9 --topic p9
- 测试脚本
    > ./bin/kafka-producer-perf-test.sh --topic p1 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic p2 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic p3 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic p4 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic p5 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic p6 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic p7 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic p8 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic p9 --num-records 1000000  --throughput 1000000 --record-size 100 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667
- 测试结果
Partition number | MB/S | Records/S
	----|------|----
	1 | 43.12  | 526381.5568
	2 | 59.28  | 698163.6358
	3 | 68.48  | 776288.8761
	4 | 66.93  | 755659.3511
    5 | 72.41  | 810932.4982
    6 | 71.73  | 807101.2581
    7 | 73.25  | 833497.0847
    8 | 69.63  | 799261.7384
    9 | 70.16  | 810309.6139
- 测试结论
由结果可知，当Partition数量小于Broker个数（3个）时，Partition数量越大，吞吐率越高，且呈线性提升。当Partition数量多于Broker个数时，总吞吐量并未有所提升，甚至还有所下降。当Partition数量为Broker数量整数倍时吞吐量明显比其它情况高。

### Throughput与Replica number的关系
- 实验条件
   3个Broker，1个Topic，3个Partition，3个Producer，消息Payload为100字节
- 测试项目
   分别测试1到3个Replica时的吞吐量
- 创建Topic
    > ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 1 --partitions 1 --topic r1
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 2 --partitions 1 --topic r2
    ./bin/kafka-topics.sh --create --zookeeper storage-1.sinocbd.local:2181,storage-2.sinocbd.local:2181,storage-3.sinocbd.local:2181 --replication-factor 3 --partitions 1 --topic r3
- 测试脚本
    > ./bin/kafka-producer-perf-test.sh --topic r1 --num-records 1000000  --throughput 1000000 --record-size 500 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667,storage-2.sinocbd.local:6667,storage-3.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic r2 --num-records 1000000  --throughput 1000000 --record-size 500 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667,storage-2.sinocbd.local:6667,storage-3.sinocbd.local:6667
    ./bin/kafka-producer-perf-test.sh --topic r3 --num-records 1000000  --throughput 1000000 --record-size 500 --producer-props bootstrap.servers=storage-1.sinocbd.local:6667,storage-2.sinocbd.local:6667,storage-3.sinocbd.local:6667
- 测试结果
Replica number | MB/S | Records/S
	----|------|----
	1 | 42.25  | 443066.0168
	2 | 43.13  | 452284.0344
	3 | 33.53  | 351617.4402
- 测试结论
由Kafka相关知识可知，随着Replica数量的增加，吞吐率应随之下降。但由数据可知吞吐率的下降并非线性下降，分析得出因为多个Follower的数据复制是并行进行的，而非串行进行；再结合之前测试结果分析得出因为单个吞吐率并未达到应有的吞吐率。

## 消费者测试
### Throughput与Consumer number的关系
- 实验条件
   3个Broker，1个Topic，6个Partition，1个Replication，消息Payload为100字节
- 测试项目
   分别测试1到3个Consumer时的集群总吞吐量
- 测试脚本
    > /bin/kafka-consumer-perf-test.sh --broker-list storage-1.sinocbd.local:6667,storage-2.sinocbd.local:6667,storage-3.sinocbd.local:6667 --threads 1 --topic D1 --messages 1000000
    ./bin/kafka-consumer-perf-test.sh --broker-list storage-1.sinocbd.local:6667,storage-2.sinocbd.local:6667,storage-3.sinocbd.local:6667 --threads 2 --topic D1 --messages 1000000
    ./bin/kafka-consumer-perf-test.sh --broker-list storage-1.sinocbd.local:6667,storage-2.sinocbd.local:6667,storage-3.sinocbd.local:6667 --threads 3 --topic D1 --messages 1000000
- 测试结果
Replica number | MB/S | nMsg/S
	----|------|----
	1 | 64.49  | 676132.522
	2 | 105.74  | 1108647.45
	3 | 106.69  | 1118568.233
- 测试结论
由数据可知，单个Consumer每秒可消费64万条消息，该数量远大于单个Producer每秒可消费的消息数量，这保证了在合理的配置下，消息可被及时处理。并且随着Consumer数量的增加，集群总吞吐量增加。


