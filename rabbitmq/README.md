# RabbitMQ

RabbitMQ 是一个开源的消息代理软件，实现了 AMQP 协议。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `rabbitmq4/` | 4.x | 最新版本 |
| `rabbitmq3/` | 3.x | 稳定版本 |
| `rabbitmq3-bitnami/` | 3.x (Bitnami) | 生产级镜像 |

## 快速启动

```bash
cd rabbitmq3 && docker-compose up -d
```

## 端口

| 端口 | 协议 | 说明 |
|------|------|------|
| 5672 | AMQP | 消息队列协议 |
| 15672 | HTTP | Management UI |
| 61613 | STOMP | STOMP协议 |
| 1883 | MQTT | MQTT协议 |
| 15692 | Prometheus | 监控指标 (v4) |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| RABBITMQ_USER | admin |
| RABBITMQ_PASSWORD | admin123456 |
| RABBITMQ_VHOST | / |

## Web管理界面

访问 http://localhost:15672

- 用户名: admin
- 密码: admin123456

## 连接示例

```bash
# Python (pika)
pip install pika
pika.ConnectionParameters('localhost', 5672, '/', pika.PlainCredentials('admin', 'admin123456'))

# Node.js (amqplib)
amqp.connect('amqp://admin:admin123456@localhost:5672')

# URL格式
amqp://admin:admin123456@localhost:5672/
```

## 常用操作

```bash
# 查看状态
docker exec rabbitmq3 rabbitmqctl status

# 查看队列
docker exec rabbitmq3 rabbitmqctl list_queues

# 查看用户
docker exec rabbitmq3 rabbitmqctl list_users

# 重置
docker exec rabbitmq3 rabbitmqctl reset
```

## 插件

默认启用插件:
- rabbitmq_management
- rabbitmq_web_stomp
- rabbitmq_mqtt

## 持久化

数据存储在 `rabbitmq_data` volume。
