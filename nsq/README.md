# NSQ

NSQ 是一个实时分布式消息平台。

## 快速启动

```bash
cd nsq && docker-compose up -d
```

## 组件

| 服务 | 端口 | 说明 |
|------|------|------|
| nsqlookupd | 4160, 4161 | 服务发现 |
| nsqd | 4150, 4151 | 消息队列 |
| nsqadmin | 4171 | 管理界面 |

## 端口

| 端口 | 服务 | 说明 |
|------|------|------|
| 4150 | nsqd TCP | 消息传输 |
| 4151 | nsqd HTTP | HTTP API |
| 4160 | nsqlookupd TCP | 查询服务 |
| 4161 | nsqlookupd HTTP | 查询API |
| 4171 | nsqadmin | Web界面 |

## Web界面

访问 http://localhost:4171

## 生产消息

```bash
# HTTP API
curl -d 'hello world' 'http://localhost:4151/pub?topic=mytopic'

# nsq_to_http
nsq_to_http --topic=mytopic --lookupd-http-address=localhost:4161 --post=http://api.example.com/webhook

# 命令行
nsq_to_file --topic=mytopic --output-dir=/tmp --lookupd-http-address=localhost:4161
```

## 消费消息

```bash
# 创建channel
curl -X POST 'http://localhost:4151/channel/create?topic=mytopic&channel=mychannel'

# HTTP消费
curl 'http://localhost:4151/stats?topic=mytopic&channel=mychannel'

# 命令行消费
nsq_stat --lookupd-http-address=localhost:4161

# 持续消费
nsq_tail --topic=mytopic --channel=mychannel --lookupd-http-address=localhost:4161
```

## Python客户端

```python
import nsq

# 生产者
def pub_message():
    writer = nsq.Writer(['localhost:4150'])
    writer.pub('mytopic', 'hello from python', callback=lambda conn, data: print(data))

# 消费者
def handler(message):
    print(message.body)
    return True

reader = nsq.Reader(
    message_handler=handler,
    lookupd_http_addresses=['localhost:4161'],
    topic='mytopic',
    channel='mychannel',
    nsqd_tcp_addresses=['localhost:4150']
)

nsq.run()
```

## Go客户端

```go
package main

import (
    "github.com/nsqio/go-nsq"
)

func main() {
    // 生产者
    producer, _ := nsq.NewProducer("localhost:4150", nsq.NewConfig())
    producer.Publish("mytopic", []byte("hello from go"))

    // 消费者
    consumer, _ := nsq.NewConsumer("mytopic", "mychannel", nsq.NewConfig())
    consumer.AddHandler(&myHandler{})
    consumer.ConnectToNSQLookupd("localhost:4161")
}

type myHandler struct{}

func (h *myHandler) HandleMessage(m *nsq.Message) error {
    println(string(m.Body))
    return nil
}
```

## 常用命令

```bash
# 查看统计
curl http://localhost:4151/stats

# 查看topic
curl http://localhost:4161/topics

# 查看channel
curl 'http://localhost:4161/channels?topic=mytopic'

# 删除topic
curl -X POST 'http://localhost:4151/topic/delete?topic=mytopic'

# 清空队列
curl -X POST 'http://localhost:4151/topic/empty?topic=mytopic'

# 空topic
curl -X POST 'http://localhost:4151/topic/pause?topic=mytopic'

# 恢复topic
curl -X POST 'http://localhost:4151/topic/unpause?topic=mytopic'
```

## 持久化

- nsq_data: NSQ数据
