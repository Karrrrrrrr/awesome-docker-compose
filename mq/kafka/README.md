# Kafka

Apache Kafka 是一个分布式流处理平台。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `kafka3/` | 3.6 + Zookeeper | 经典模式 |
| `kafka3-bitnami/` | 3.6 (Bitnami) | 生产级镜像 |
| `kafka-kraft/` | 3.6 KRaft | 无Zookeeper模式 |

## 快速启动

```bash
# Zookeeper模式
cd kafka3 && docker-compose up -d

# KRaft模式 (推荐)
cd kafka-kraft && docker-compose up -d
```

## 端口

| 服务 | 端口 |
|------|------|
| Kafka | 9092 |
| Kafka (External) | 9094 |
| Zookeeper | 2181 |
| Kafka UI | 9091 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| KAFKA_PORT | 9092 |
| KAFKA_EXTERNAL_PORT | 9094 |
| ZOOKEEPER_PORT | 2181 |
| KAFKA_UI_PORT | 9091 |

## 常用操作

```bash
# 创建Topic
docker exec kafka3 kafka-topics.sh --create --topic test --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1

# 查看Topics
docker exec kafka3 kafka-topics.sh --list --bootstrap-server localhost:9092

# 生产消息
docker exec -it kafka3 kafka-console-producer.sh --topic test --bootstrap-server localhost:9092

# 消费消息
docker exec -it kafka3 kafka-console-consumer.sh --topic test --from-beginning --bootstrap-server localhost:9092

# 查看消费者组
docker exec kafka3 kafka-consumer-groups.sh --list --bootstrap-server localhost:9092
```

## Kafka UI

访问 http://localhost:9091 使用 Kafka UI 管理界面。

## 持久化

- Kafka: `kafka_data` volume
- Zookeeper: `zookeeper_data` volume
