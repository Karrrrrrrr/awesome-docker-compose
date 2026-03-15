# EMQX

EMQX 是一个开源的 MQTT 消息代理，专为物联网设计。

## 版本

- `emqx5/`: EMQX 5.x

## 快速启动

```bash
cd emqx5 && docker-compose up -d
```

## 端口

| 端口 | 协议 | 说明 |
|------|------|------|
| 1883 | MQTT | TCP端口 |
| 8883 | MQTTS | SSL端口 |
| 8083 | WebSocket | WS端口 |
| 8084 | WebSocket Secure | WSS端口 |
| 18083 | Dashboard | 管理界面 |
| 8081 | API | REST API |

## Dashboard

访问 http://localhost:18083

| 用户 | 密码 |
|------|------|
| admin | admin123456 |

## 客户端连接

### MQTT连接

```
Broker: localhost
Port: 1883
Client ID: client-001
Username: (可选)
Password: (可选)
```

### WebSocket连接

```
URL: ws://localhost:8083/mqtt
Protocol: mqtt
```

## 常用命令

```bash
# 查看状态
docker exec emqx5 emqx ctl status

# 查看节点信息
docker exec emqx5 emqx ctl broker

# 查看客户端列表
docker exec emqx5 emqx ctl clients list

# 查看订阅列表
docker exec emqx5 emqx ctl subscriptions list

# 查看主题
docker exec emqx5 emqx ctl topics list
```

## REST API

```bash
# 获取节点信息
curl -i -X GET "http://localhost:8081/api/v5/nodes"

# 获取客户端列表
curl -i -X GET "http://localhost:8081/api/v5/clients" \
  -u admin:admin123456

# 发布消息
curl -i -X POST "http://localhost:8081/api/v5/publish" \
  -u admin:admin123456 \
  -H "Content-Type: application/json" \
  -d '{"topic": "test/topic", "payload": "hello", "qos": 1}'
```

## 客户端示例

### Python

```python
import paho.mqtt.client as mqtt

client = mqtt.Client()
client.connect("localhost", 1883, 60)

# 发布
client.publish("test/topic", "Hello MQTT")

# 订阅
def on_message(client, userdata, msg):
    print(f"{msg.topic}: {msg.payload}")

client.on_message = on_message
client.subscribe("test/#")
client.loop_forever()
```

### JavaScript

```javascript
const mqtt = require('mqtt')
const client = mqtt.connect('mqtt://localhost:1883')

client.on('connect', () => {
  client.subscribe('test/#')
  client.publish('test/topic', 'Hello MQTT')
})

client.on('message', (topic, message) => {
  console.log(`${topic}: ${message}`)
})
```

## 配置文件

- `emqx.conf`: 主配置文件
- `loaded_plugins`: 启用插件列表

## 持久化

- emqx_data: EMQX数据
- emqx_log: 日志
