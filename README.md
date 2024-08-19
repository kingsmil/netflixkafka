Found that wurstmeister/zookeeper does not support ARM64 architecture, swapping to bitnami/zookeeper and kafka

It should be noted that bitnami/kafka is meant to run standalone as it runs on kRaft Architecture, which does not require it.

Will be following the Zookeeper/Kafka Architecture as it is industry standard for this project

```docker-compose up -d```

```
docker exec -it <kafka container name> /bin/bash  
```
```
kafka-console-producer.sh --producer.config /opt/bitnami/kafka/config/producer.properties --bootstrap-server kafka:9092 --topic test
kafka-console-consumer.sh --consumer.config /opt/bitnami/kafka/config/consumer.properties --bootstrap-server kafka:9092 --topic test --from-beginning
```

# Choosing spark or flink
Flink is better for streaming data that requires less than 10ms
- Therefore we choose spark as it is generally better when 500ms latency is alright

Chose slave-master architecture as it is more scalable. Useful to learn for future projects.

Able to access webUI

----

Submitting a scala job

1. creating the jar file for spark to run and sending it
```
sbt package
docker cp target/scala-2.12/simple-project_2.12-1.0.jar toypipeline-spark-1:/opt/bitnami/spark
docker exec -it toypipeline-spark-1 /bin/bash
```
2. executing on the spark container
```
spark-submit --class "SimpleApp" --master local[4] simple-project_2.12-1.0.jar
```

