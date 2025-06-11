# Apache Kafka Notes

## Kafka Flow
![Kafka Flow](https://github.com/smranjanmishra/Apache-Kafka/blob/main/Kafka%20Flow/The%20way%20Kafka%20Works.png)
</br>
Client sends data → Server (Producer) pushes to Kafka Topic → Kafka stores → Worker (Consumer) reads → processes → saves to DB → (optional response to client).

Kafka supports 2 models:
- **Pub/Sub Model**
- **Queue with Partitioning**
---

## Why Kafka?
- **Scalability** – Handles massive data volumes easily
- **Persistence** – Durable storage on disk
- **Replayability** – Can replay old events using offsets
- **High Throughput & Low Latency** – Fast and efficient processing
- **Fault Tolerant** – Works even if some nodes fail (via replication)

---

## Kafka vs RabbitMQ

| Feature              | Apache Kafka                          | RabbitMQ                                 |
|----------------------|----------------------------------------|------------------------------------------|
| Architecture         | Distributed event streaming platform   | Message broker with queues               |
| Message Model        | Pub-Sub (log-based)                    | Producer-Consumer (queue-based)          |
| Message Persistence  | Configurable retention period          | Deleted after consumption (unless stored)|
| Scalability          | Horizontally scalable                  | Complex                                  |
| Throughput           | High                                   | Lower                                    |
| Latency              | Higher                                 | Low                                      |
| Message Replay       | Supported                              | Not built-in                             |
| Delivery Guarantee   | At-least-once, exactly-once            | At-most-once, at-least-once, exactly-once|
| Use Case             | Real-time streaming, analytics         | Task queues, transactional messaging     |
| Routing              | Topic-based                            | Exchange-based                           |
| Protocol Support     | TCP                                    | AMQP, MQTT, STOMP, etc.                  |

---

## When to Use Kafka
- Real-time analytics – e.g., Uber’s ride tracking
- Event-driven microservices – e.g., Netflix’s user activity
- Big data ingestion – e.g., LinkedIn’s activity stream

**Key Takeaway:** Kafka is a scalable event streaming platform, not just a messaging queue.

---

## Uber Example (Diagram Flow)
![Uber Structure](https://github.com/smranjanmishra/Apache-Kafka/blob/main/Uber%20Structure%20Diagram/Uber%20Structure.png)
Rider sends a ride request → API Gateway sends to Kafka → Kafka sends to Matching, Trip, Billing → Services process and save to DB → Notifications sent to rider.

---

## Kafka Cluster and ZooKeeper

- A Kafka Cluster may consist of 3 brokers (e.g., 1, 2, 3). If one broker goes down, another takes over.
- **ZooKeeper** manages the Kafka Cluster, acts as the brain.

---

## Core Components of Apache Kafka

1. **Kafka Broker** – Server storing data.
2. **Producer** – Sends data to Kafka.
3. **Topic** – Feed name or category where records are stored.
4. **Consumer & Consumer Group** – Applications reading the data.
5. **Zookeeper** – Manages brokers, metadata, and leader election.

---

## Important Kafka Concepts

- **Topic Partition** – Topics are divided for scalability.
- **Consumer Group** – Consumers that subscribe together to a topic.
- **Node** – One computer in the cluster.
- **Replicas** – Backup copies of partitions.
- **Offset** – Unique ID of a message in a partition.

---

## Why Kafka is Needed

- Real-Time Processing
- Fault-Tolerant
- Horizontally Scalable
- Enables Event-Driven Architecture

---

## How Kafka Works

1. Producers send data
2. Kafka stores in Topics (split into partitions)
3. Consumers read from Topics (using offsets)
4. Kafka balances the load via ZooKeeper
5. Data processed/stored/triggered

---

## Kafka as Data Processor

### 1. Event Streaming (Pub/Sub)
- Multiple consumers read from same topic.
- Example: Live stock market data.

### 2. Message Queue (Consumer Group)
- One message → one consumer (load-balanced).
- Example: Uber assigning rides.

### 3. Batch Processing
- Messages stored → processed later (e.g., Spark).
- Example: E-commerce analytics.

### 4. Hybrid Model
- Real-time + batch (e.g., fraud detection).

---

## Use Cases

- Real-time Analytics
- Event-Driven Apps
- Log Aggregation
- Stream Processing
- Data Integration

---

## Companies Using Kafka

| Company       | Use Case                                    |
|---------------|----------------------------------------------|
| LinkedIn      | Real-time activity streams                   |
| Netflix       | Monitoring, recommendations                  |
| Twitter       | Live tweet analytics                         |
| Uber          | Ride tracking, billing                       |
| Airbnb        | Pricing, user analytics                      |
| Spotify       | Music data analytics                         |
| Pinterest     | Event logging, recommendations               |
| Walmart       | Inventory, fraud detection                   |
| Box           | Real-time analytics                          |
| Goldman Sachs | Financial data streaming                     |

---

## Benefits of Kafka

- Handles large data volumes
- Fault-tolerant
- Real-time processing
- Works with any data format
- Easy integration
- Strong community

---

## Limitations of Kafka

- Complex setup
- Expensive storage
- Order guarantees only within partition
- Needs external processors
- High resource consumption
- Not ideal for very small messages

---

## Features of Kafka

- Scalability via partitions
- Fault Tolerance with replication
- Flexibility (any data type)
- Offset Management for consumers

---

## Apache Tools with Kafka

- **ZooKeeper** – Cluster coordination
- **Avro** – Data serialization
- **Flink** – Real-time processing
- **Spark** – Batch + real-time analytics
- **Hadoop** – Long-term storage
- **Storm** – Low-latency event processing
- **Camel** – Data routing
- **NiFi** – Automated data pipelines

---

## Kafka Topics Management

### Create a New Topic
```bash
kafka-topics --create \
  --topic <topic-name> \
  --bootstrap-server localhost:9092 \
  --partitions <num-partitions> \
  --replication-factor <replication-factor>
```

- `--topic` → Name of the topic
- `--bootstrap-server` → Kafka broker address
- `--partitions` → Number of partitions
- `--replication-factor` → Replication factor for high availability

---

###  List All Topics
```bash
kafka-topics --list --bootstrap-server localhost:9092
```

---

###  Describe a Topic
```bash
kafka-topics --describe --topic <topic-name> --bootstrap-server localhost:9092
```

---

###  Delete a Topic
```bash
kafka-topics --delete --topic <topic-name> --bootstrap-server localhost:9092
```

> Ensure `delete.topic.enable=true` in Kafka config

---

## Producing & Consuming Messages

### Start a Producer
```bash
kafka-console-producer --topic <topic-name> --bootstrap-server localhost:9092
```
---

### Start a Consumer
```bash
kafka-console-consumer \
  --topic <topic-name> \
  --bootstrap-server localhost:9092 \
  --from-beginning
```

---

## Managing Consumer Groups

### List All Consumer Groups
```bash
kafka-consumer-groups --list --bootstrap-server localhost:9092
```

### Describe a Consumer Group
```bash
kafka-consumer-groups \
  --describe \
  --group <group-name> \
  --bootstrap-server localhost:9092
```

### Reset Consumer Group Offset
```bash
kafka-consumer-groups \
  --reset-offsets \
  --group <group-name> \
  --topic <topic-name> \
  --to-earliest \
  --bootstrap-server localhost:9092 \
  --execute
```

---

## Kafka Cluster Management

### Check Broker API Versions
```bash
kafka-broker-api-versions --bootstrap-server localhost:9092
```

### Describe Cluster (Broker Info)
```bash
kafka-cluster --describe --bootstrap-server localhost:9092
```

---

## Admin Commands

### Start Zookeeper
```bash
zookeeper-server-start.sh config/zookeeper.properties
```

### Start Kafka Broker
```bash
kafka-server-start.sh config/server.properties
```

### Stop Kafka Broker
```bash
kafka-server-stop.sh
```

### Stop Zookeeper
```bash
zookeeper-server-stop.sh
```

---

## Kafka Performance Testing

### Test Producer Performance
```bash
kafka-producer-perf-test \
  --topic <topic-name> \
  --num-records 100000 \
  --record-size 100 \
  --throughput 1000 \
  --bootstrap-server localhost:9092
```

### Test Consumer Performance
```bash
kafka-consumer-perf-test \
  --topic <topic-name> \
  --messages 100000 \
  --bootstrap-server localhost:9092
```

---

## Debugging & Troubleshooting

### Check Kafka-Zookeeper Connection
```bash
echo dump | nc localhost 2181
```

### List Active Kafka Topics
```bash
kafka-topics --list --bootstrap-server localhost:9092
```

### List Active Consumer Groups
```bash
kafka-consumer-groups --list --bootstrap-server localhost:9092
```

---

## Final Notes

- Ensure Kafka and Zookeeper are running before using these commands.
- Replace `localhost:9092` with your actual Kafka broker address if needed.

---

## Conclusion
Apache Kafka is a robust real-time event streaming platform. From real-time analytics to log aggregation and event-driven architectures, it supports scalable, fault-tolerant, and flexible data pipelines integrated with a range of big data tools.
