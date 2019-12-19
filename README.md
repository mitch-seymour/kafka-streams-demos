# kafka-streams-demos

# Quickstart
Run this Kafka Streams app in the Confluent Platform sandbox by executing the following commands:
```bash
# clone this repo
$ git clone https://github.com/mitch-seymour/kafka-streams-demos.git
$ cd kafka-streams-demos

# mount this code in the Confluent Platform sandbox
$ docker run --name sandbox \
  -v "$(pwd)":/app \
  -w /app/hello-streams \
  -ti magicalpipelines/cp-sandbox:5.3.0 \
  bash -c "confluent local start kafka; bash"

# wait for kafka and zookeeper to start
# ...

# create the source topic
$ kafka-topics \
  --bootstrap-server localhost:9092 \
  --topic hello-world \
  --replication-factor 1 \
  --partitions 4 \
  --create

# run the Kafka Streams app
./gradlew runProcessorApi

```

Now, in another tab, use `ksql-datagen` to produce some dummy data:

```bash
$ docker exec -ti sandbox \
  ksql-datagen quickstart=users topic=hello-world format=delimited
```

Switch back to the first tab (where you're running the Kafka Streams app), and you should see output similar to the following:

```bash
Starting streams
(Processor API) Hello, 1503275958331,User_2,Region_5,OTHER
(Processor API) Hello, 1507181088524,User_8,Region_6,OTHER
```
