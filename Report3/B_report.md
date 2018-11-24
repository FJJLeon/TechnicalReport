# 设置kafka集群 #

## 过程
1. 准备好多台linux机器和已下载好的kafka压缩包
2. 创建目录并下载安装软件
```
#创建目录
cd /opt/
mkdir kafka #创建项目目录
cd kafka
mkdir kafkalogs #创建kafka消息目录，主要存放kafka消息
#下载软件
wget  http://apache.opencas.org/kafka/0.9.0.1/kafka_2.11-0.9.0.1.tgz
#解压软件
tar -zxvf kafka_2.11-0.9.0.1.tgz
```
3. 进入config目录，使用kafka自带的zk集群，修改配置文件。
```
#broker.id=0  每台服务器的broker.id都不能相同
#hostname
host.name=192.168.7.100
#在log.retention.hours=168 下面新增下面三项
message.max.byte=5242880
default.replication.factor=2
replica.fetch.max.bytes=5242880
#设置zookeeper的连接端口
zookeeper.connect=192.168.7.100:12181,192.168.7.101:12181,192.168.7.107:12181
```
4. 启动kafka集群并测试（创建Topic来验证是否创建成功）
```
#创建Topic
./kafka-topics.sh --create --zookeeper 192.168.7.100:12181 --replication-factor 2 --partitions 1 --topic shuaige
#解释
--replication-factor 2   #复制两份
--partitions 1 #创建1个分区
--topic #主题为shuaige
'''在一台服务器上创建一个发布者'''
#创建一个broker，发布者
./kafka-console-producer.sh --broker-list 192.168.7.100:19092 --topic shuaige
'''在一台服务器上创建一个订阅者'''
./kafka-console-consumer.sh --zookeeper localhost:12181 --topic shuaige --from-beginning
```

## 生产者与消费者
1. 执行生产者命令
```
/bin/kafka-console-producer.sh --broker-list server01:9092 --topic test
```
2. 执行消费者命令
```
# --from-beginning：表示从生产的开始获取数据
./bin/kafka-console-consumer.sh --zookeeper server01:2181 --from-beginning --topic test
```