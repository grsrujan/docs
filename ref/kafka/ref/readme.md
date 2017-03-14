Apache Kafka is a distributed publish-subscribe messaging system and a robust queue that can handle a high volume of data and enables you to pass messages from one end-point to another. Kafka is suitable for both offline and online message consumption.

Point to point messaging systems
In a point-to-point system, messages are persisted in a queue. One or more consumers can consume the messages in the queue, but a particular message can be consumed by a maximum of one consumer only. Once a consumer reads a message in the queue, it disappears from that queue.

Publish-subscribe messaging systems
In the publish-subscribe system, messages are persisted in a topic. Unlike point-to-point system, consumers can subscribe to one or more topic and consume all the messages in that topic. In the Publish-Subscribe system, message producers are called publishers and message consumers are called subscribers. A real-life example is Dish TV, which publishes different channels like sports, movies, music, etc., and anyone can subscribe to their own set of channels and get them whenever their subscribed channels are available.

To start kafka

Bin/kafka-server-start.sh config/server.properties

Create topic

Bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --topic data

Producer
Bin/kafka-console-producer.sh --broker-list localhost:9092 --topic data

List all topics
Bin/kafka-topics.sh --list --zookeeper localhost:2181

Consumer
Bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic data --from-beginning

Modifying a Topic

bin/kafka-topics.sh —zookeeper localhost:2181 --alter --topic topic_name 
--parti-tions count

Deleting a Topic

bin/kafka-topics.sh --zookeeper localhost:2181 --delete --topic topic_name
