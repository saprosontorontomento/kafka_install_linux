# Kafka install linux
```
sudo apt update  
```
## download the latest Apache Kafka binary files
[kafka download website](https://kafka.apache.org/downloads)
## Scala version 2.13 kafka version 3.2.1 
```
wget https://dlcdn.apache.org/kafka/3.2.1/kafka_2.13-3.2.1.tgz
```
## Then extract the downloaded archive file and place them under /usr/local/kafka directory.
```
tar xzf kafka_2.13-3.2.1.tgz
```
```
sudo mv kafka_2.13-3.2.1 /usr/local/kafka
```
## Create Systemd Startup Scripts
```
sudo nano /etc/systemd/system/zookeeper.service 
```
```
[Unit]
Description=Apache Zookeeper server
Documentation=http://zookeeper.apache.org
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Service]
Type=simple
ExecStart=/usr/local/kafka/bin/zookeeper-server-start.sh /usr/local/kafka/config/zookeeper.properties
ExecStop=/usr/local/kafka/bin/zookeeper-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
```
```
sudo nano /etc/systemd/system/kafka.service 
```
```
[Unit]
Description=Apache Kafka Server
Documentation=http://kafka.apache.org/documentation.html
Requires=zookeeper.service

[Service]
Type=simple
Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
ExecStart=/usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties
ExecStop=/usr/local/kafka/bin/kafka-server-stop.sh

[Install]
WantedBy=multi-user.target
```
## Reload
```
sudo systemctl daemon-reload 
```
## Start zookeeper & kafka
```
systemctl start zookeeper 
systemctl start kafka 
```
## Stop zookeeper & kafka
```
systemctl start zookeeper
```
```
systemctl start kafka 
```
## or start interactive
```
./zookeeper-server-start.sh ../config/zookeeper.properties 
```
```
./kafka-server-start.sh ../config/server.properties 
```
## Status
```
systemctl status zookeeper
```
```
systemctl status kafka
```
##  Create a Topic in Kafka
```
cd /usr/local/kafka/bin
```
```
./kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic NAME
```
## Check Topic
```
./kafka-topics.sh --list --bootstrap-server localhost:9092
```
## Delete Topic
```
./kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic NAME
```
##  Send and Receive Messages in Kafka
###  добавляем сообщения
```
./kafka-console-producer.sh --broker-list localhost:9092 --topic NAME
```
### читаем сообщения
```
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NAME --from-beginning
```
##  Group
### создать группу
```
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NAME --from-beginning --group NAME_GROUP
```
### показать состояние
```
./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group NAME_GROUP --describe
```
