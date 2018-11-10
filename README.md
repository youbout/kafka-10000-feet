# Presentation

This repo contains the steps to follow to produce the example in the Kafka 10000 Feet View presentation. You can find the presentation [here](https://www.slideshare.net/younessx01/kafka-10000-feet-view-122685158).

This example was run on Ubuntu distribution. 

## Installation

First you need to Download Kafka Binaries. Download the latest version [here](https://kafka.apache.org/downloads).

```bash
tar xzvf "Kafka Downloaded File"

cd "unzipped folder"
```

That's it. Now we can start exploring Kafka commands.

## Usage

#### Running Zookeeper
```bash
bin/zookeeper-server-start.sh config/zookeeper.properties

```

#### Running the Brokers
In our example we have 3 Brokers. So we need to have 3 instances of the broker configuration file:

```bash
cp config/server.properties config/server-1.properties
cp config/server.properties config/server-2.properties
```

Now Edit the new files server-1.properties and server-2.properties, and change these properties :

```bash
"Here put an id. 1 for server-1 and 2 for server-2"
broker.id= 

"Uncomment this line first. Then increment the port number: 9093, 9094" 
listeners= PLAINTEXT://:9092 

"Change the value of this directory. For example kafka-logs-1, kafka-logs-2. 
We don't want All Brokers to write to the same directory"
log.dirs=/tmp/kafka-logs

```

Now let's run our brokers.

```bash
bin/kafka-server-start.sh config/server.properties
bin/kafka-server-start.sh config/server-1.properties
bin/kafka-server-start.sh config/server-2.properties
```

#### Creating a Topic
Our 3 Brokers are now up and running. Let's start creating some topics. In our example we have one topic with 3 partitions and replication factor of 3.

Let's name our topic "**orders**"
```bash
bin/kafka-topics.sh --zookeeper localhost:2181 --create --topic orders --partitions 3 --replication-factor 3
```

#### Creating Consumers

In our example we have **2** consumer groups, one has 3 consumers and the other has 2. Let's create them. 
Open 5 terminals, and execute theses following commands. One command in a terminal: 

```bash
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092,localhost:9093,localhost:9094  --topic orders --group group-1
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092,localhost:9093,localhost:9094  --topic orders --group group-1
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092,localhost:9093,localhost:9094  --topic orders --group group-1

bin/kafka-console-consumer.sh --bootstrap-server localhost:9092,localhost:9093,localhost:9094  --topic orders --group group-2
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092,localhost:9093,localhost:9094  --topic orders --group group-2
```

#### Creating the Producer
Now that we have our consumers listening for messages in different partitions. Let's create a Producer and start writing some messages. Open a new terminal an type in this command:

```bash
bin/kafka-console-producer.sh --broker-list localhost:9092,localhost:9093,localhost:9094 --topic orders
```

Now start writing some messages and watch the consumers terminals how they are behaving.
